# 云朵输入法脚本仓库

### 本仓库的脚本基于 Lua 语言设计，并使用 JSON 文件作为前端界面设计。

### 如何使用？

打开「云朵输入法设置 → 脚本」，点「在线获取脚本」，列表里每一行的「获取」会下载并安装对应的脚本及配套文件。
装完点「重启输入法服务」生效。也可以用 `git clone` 本仓库，按下面的「目录」把文件手动放进安装目录的 `Scripts\`。

### 目录

- `index.json`：在线获取用的索引（格式见下）
- `src\小牛候选翻译`：小牛翻译（写候选窗底部一行），含 `md5.lua`
- `src\小牛整句翻译（独立窗口）`：小牛翻译独立窗口，含 `md5.lua` 与布局 JSON
- `src\在线查询（有道+汉典）`：在线查询窗口，含布局 JSON
- `lua.md`：脚本接口文档

### 新增脚本

在 `src\` 下建一个目录放脚本与配套文件，然后在 `index.json` 的 `scripts` 里加一项：

```json
{
  "id": "唯一标识",
  "name": "显示名",
  "description": "一句话介绍",
  "files": [
    { "path": "src/目录/main.lua", "target": "script" },
    { "path": "src/目录/shared.lua", "target": "shared" },
    { "path": "src/目录/window.json", "target": "layout" },
    { "path": "src/目录/native.dll", "target": "dll" }
  ]
}
```

`target` 决定文件装到安装目录 `Scripts\` 的哪里：

| target | 安装位置 | 约束 |
|---|---|---|
| `script` | `Scripts\` 根目录 | 必须是 `.lua`；装进去才会被输入法加载 |
| `shared` | `Scripts\shared\` | 共享 Lua |
| `layout` | `Scripts\layouts\` | 布局 JSON |
| `dll` | `Scripts\dlls\` | 动态库 |

安装时只取 `path` 的文件名（目录结构只用于本仓库组织源码），所以同一个脚本里两个文件不能落到同一位置。
客户端会先下载全部文件再写盘：目标已存在且内容相同就跳过，内容不同则整份不装，提示用户先删旧文件。
索引版本固定为 `1`；`id` 不能重复，`name` 不能为空。
