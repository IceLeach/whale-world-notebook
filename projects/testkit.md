# testkit（插件实机测试环境）

> 本项目专属用法。通用文档在 `testkit\README.md`（人）和 `testkit\AGENTS.md`（AI）。

## 位置

- 项目：`D:\EX DSH\whale-world\testkit`
- 隔离 home：`testkit\home`（含登录态副本，已 gitignore）
- 被测插件：`D:\EX DSH\whale-world\plugins\packages\<name>`

## 本机路径

- DSH 可执行：`D:\DeepSeek Harness\DeepSeek Harness.exe`
- DSH CLI：`D:\DeepSeek Harness\resources\app.asar\dsh\node_modules\@deepseek-ai\dsh-desktop-host\lib\cli.js`
- 运行器默认用这两个路径；换机器用 `DSH_EXE` / `DSH_CLI` 覆盖。

## 常用命令（从 whale-world 根执行）

```powershell
# 初始化（一次）
powershell -NoProfile -ExecutionPolicy Bypass -File testkit\init.ps1

# 安装 hello 插件
powershell -NoProfile -ExecutionPolicy Bypass -File testkit\install-plugin.ps1 -Name hello

# 安装示例插件（带 ms 依赖）
powershell -NoProfile -ExecutionPolicy Bypass -File testkit\install-plugin.ps1 -Name hello-plugin -Source example

# 跑场景
node testkit\runner\run.mjs --scenario hello
node testkit\runner\run.mjs --scenario example\scenarios\hello.json
```

## 已验证

- hello 场景 6/6 通过：`tool_call: whale_hello` → `tool_result: hello from dsh-whale-hello` → `final`。

## 本机已知问题

- 当前工具是 PowerShell 5.1，执行策略受限：脚本必须用 `powershell -NoProfile -ExecutionPolicy Bypass -File`。
- PowerShell 5.1 的 `1>` 重定向写 UTF-16、`Get-Content` 默认 GBK，都会破坏中文/JSON；运行器因此用 Node + 文件描述符。
- `cmd /c` 的管道 `|` 被沙箱拒，`<` 输入重定向可以。
- `$home` 是 PowerShell 只读自动变量，变量名不要用它。
- DSH 沙箱禁止 Node 的 `child_process` 用 pipe（`EPERM`）；用文件描述符可绕过。

## 与当前 Harness 的隔离

- 独立 `DSH_HOME`（`testkit/home`）、独立 profile（`headless`）、独立进程、不开端口。
- 复用的是桌面版登录态（`~/.dsh/.credentials.yaml` 的副本）。

## 场景清单

| 场景 | 测什么 | 状态 |
|---|---|---|
| `hello` | 插件加载、工具注册、调用与返回 | 6/6 通过 |
| `example/scenarios/hello.json` | 示例插件 + npm 依赖（`ms`） | 6/6 通过 |
| `example/scenarios/mono.json` | monorepo 结构：插件依赖共享包 | 6/6 通过 |

## 示例

- `example/hello-plugin/`：普通插件，带 npm 依赖 `ms`。
- `example/monorepo/`：workspace 结构，`packages/core` 是共享包，`packages/mono-plugin` 依赖它。

## 本机已知问题

- 当前工具是 PowerShell 5.1，执行策略受限：脚本必须用 `powershell -NoProfile -ExecutionPolicy Bypass -File`。
- PowerShell 5.1 的 `1>` 重定向写 UTF-16、`Get-Content` 默认 GBK，都会破坏中文/JSON；运行器因此用 Node + 文件描述符。
- `cmd /c` 的管道 `|` 被沙箱拒，`<` 输入重定向可以。
- `$home` 是 PowerShell 只读自动变量，变量名不要用它。
- DSH 沙箱禁止 Node 的 `child_process` 用 pipe（`EPERM`）；用文件描述符可绕过。
- 插件注册工具必须声明 `output: { schema, render }`，否则报 `tool "x" must declare output`，插件不会激活。
- 隔离 profile 的 `installed-plugins.txt` 存的是**包名**（不是目录名）；旧目录名混进去会插入不存在的包。
- `npm install` 要加 `--cache testkit\.npm-cache`，否则 npm 写用户目录被沙箱拒。
- PowerShell 5.1 的 `@() + 'x'` 会退化成字符串，拼接包名要用 `@('core') + $arr` 或数组字面量；`$var = if (...) {...} else {...}` 这种表达式写法在 5.1 里会报语法错。
- `install-plugin.ps1` 支持 `-Source` 指向任意目录，并自动识别 `packages/` 子目录（monorepo）；共享包（没有 `dsh.bundle.patch`）只复制、不插进 patch。
