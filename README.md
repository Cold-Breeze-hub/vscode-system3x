# 适用于 Visual Studio Code 的 System 3.x 扩展

该扩展为 AliceSoft 的 System 1-3 和 System 3.x 语言添加了支持。

![Screenshot](images/debugger.png)

## 功能特性

- `.adv` 源文件的语法高亮
- 反编译与编译：
  - System 3.x 游戏（使用附带的 [xsys35c]）
  - System 1-3 游戏（使用附带的 [sys3c]）
- 调试：
  - System 3.x 游戏（使用 [xsystem35-sdl2]）
  - System 1-3 游戏（使用 [system3-sdl2]）

以下功能仅适用于 System 3.x 游戏：

- 鼠标悬停在命令名上时显示文档说明
- 函数的[跳转到定义](https://code.visualstudio.com/docs/editor/editingevolved#_go-to-definition)

## 前置要求

- 对于 System 3.x 相关功能：
  - 要查看命令文档，需要 [System 3.9 SDK](https://web.archive.org/web/20021018163909/http://www.alicesoft.co.jp/support/sys39agr.html)。
  - 要使用调试功能，需要 [xsystem35-sdl2]（2.0.0 或更高版本）。
- 对于 System 1-3 相关功能：
  - 要使用调试功能，需要 [system3-sdl2]（1.3.0 或更高版本）。

## 快速入门

满足前置要求后，按照以下步骤开始：

1. 安装[本扩展](https://marketplace.visualstudio.com/items?itemName=kichikuou.system3x)。
2. 打开包含游戏文件的文件夹：
   - 对于 System 3.x 游戏：包含 `*SA.ALD` 文件的文件夹
   - 对于 System 1-3 游戏：包含 `ADISK.DAT` 文件的文件夹
3. 打开命令面板（<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> 或 <kbd>F1</kbd>），输入 `system3x`。从出现的列表中选择 `System3x: Decompile`。反编译后的源文件将保存在 `src` 文件夹中，扩展会自动打开第一个 `.adv` 文件。
4. 调试设置：
   - System 3.x 游戏（使用 [xsystem35-sdl2]）：
     - 在 Windows 上，将 `xsystem35.exe` 复制到与 `*.ALD` 文件相同的文件夹中。
     - 在其他平台上，请遵循 [xsystem35-sdl2] 的安装说明。
   - System 1-3 游戏（使用 [system3-sdl2]）：
     - 在 Windows 上，将 `system3.exe` 及其 DLL 文件复制到与 `ADISK.DAT` 相同的文件夹中。
     - 在其他平台上，请遵循 [system3-sdl2] 的安装说明。
5. 按 <kbd>F5</kbd> 开始调试。

## 功能详情

### 反编译

`System3x: Decompile` 命令同时适用于 System 3.x 和 System 1-3 游戏：

- 对于 System 3.x：反编译工作区文件夹中的 `*.ALD` 和 `System39.ain` 文件
- 对于 System 1-3：反编译工作区文件夹中的 `?DISK.DAT` 和 `AG00.DAT` 文件

反编译后的源文件保存在 `src` 子文件夹中。

### 编译

默认情况下，`开始调试` 命令（<kbd>F5</kbd>）会自动使用 `src` 文件夹中的源文件重新构建游戏。

若要仅构建游戏而不运行，请使用 `运行生成任务` 命令（<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>B</kbd>），然后选择 `xsys35c: build`。

### 运行

如果 `launch.json` 文件不存在，按 <kbd>F5</kbd> 将使用默认设置启动相应的游戏引擎。但这仅在当前选项卡中打开了 `.adv` 文件时才有效。

为了使 <kbd>F5</kbd> 始终可用，或者自定义启动设置，请从 `运行` 菜单中选择 `添加配置`。这将生成如下所示的 `launch.json` 文件：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "xsystem35",
            "request": "launch",
            "name": "Debug",
            "program": "${config:system3x.xsystem35Path}",
            "runDir": "${workspaceFolder}",
            "srcDir": "${workspaceFolder}/src",
            "logLevel": 2,
            "stopOnEntry": false,
            "preLaunchTask": "xsys35c: build"
        }
    ]
}
```

鼠标悬停在属性上会显示其描述。

例如，要启动游戏但不构建，请注释掉 `"preLaunchTask": "xsys35c: build"` 这一行。

### 调试

请参阅 VS Code 中的[调试](https://code.visualstudio.com/docs/editor/debugging)文档，了解如何使用调试器。

调试器支持以下适用于所有游戏的操作：

- [断点](https://code.visualstudio.com/docs/editor/debugging#_breakpoints)
- [单步执行](https://code.visualstudio.com/docs/editor/debugging#_debug-actions)
- [数据检查](https://code.visualstudio.com/docs/editor/debugging#_data-inspection)
- 调色板查看器（位于侧边栏的“运行和调试”视图中）

仅适用于 System 3.x 游戏的附加功能：

- [条件断点](https://code.visualstudio.com/docs/editor/debugging#_advanced-breakpoint-topics)
- [调试控制台 REPL](https://code.visualstudio.com/docs/editor/debugging#_debug-console-repl)

## 扩展设置

要访问您的设置，请打开设置编辑器（`Ctrl+,` 或 `Cmd+,`），然后搜索 `system3x`。

[xsys35c]: https://github.com/kichikuou/xsys35c
[sys3c]: https://github.com/kichikuou/sys3c
[xsystem35-sdl2]: https://github.com/kichikuou/xsystem35-sdl2
[system3-sdl2]: https://github.com/kichikuou/system3-sdl2
