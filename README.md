# 🌠 Neo Code (Windows Native Edition)

> 🚀 **The Ultimate Windows Adaptation of Claude Code CLI**

Neo Code 是基于 Anthropic 官方的强大 AI 代理运行器 [Claude Code](https://github.com/anthropics/claude-code) 经过底层改造的 **全血·无限制·原生适配 Windows** 特别版。

**🙏 感谢声明：**
> 饮水思源，**感谢 Anthropic 与 Claude 团队的伟大开源与技术探索！** 
> 本项目并非独立的从零造轮子，而是在官方出色的底座之上生长的延伸分支。所有的核心大模型路由、视觉组件以及工具链底层逻辑均来自官方源代码。感谢 Claude 开源之恩，使我们得以在此基础上完善它在 Windows 侧的生态体验！

---

## 🆚 本项目与官方 Claude Code 的关系

Neo Code 是官方原版的一个**原生兼容拦截补丁版 (Patch Fork)**。

官方原版 `claude-code` 对命令安全机制高度依赖于 `tree-sitter-bash`（AST 抽象解析器）来校验你输入的命令或大模型生成的复杂指令。然而，由于该解析器缺乏对 Windows 的 PowerShell 原生语法的支持（如复杂的管道表达式或命令扩展），每当输入遇到非类 Unix 的标准语句时，它会立刻触发 `too-complex` 或者 `parse-unavailable` 安全报警。这导致在使用官方原版时，用户经常会陷入 **“无法解析执行”、“频繁弹窗请求权限”** 的死循环中，体验深受制约。

**在 Neo Code 中，我们做了什么？**
我们直接在底层二进制代码中，对 `parseForSecurity` 这一套件的调用点进行了**平台嗅探与动态拦截热注入**：
- 当识别为 `win32` 系统时，切断会强制报错的树解析机制，直接应用纯净的命令拆解与权限规则白名单系统。
- 如此一来，**100% 保留官方原生功能的前提下**，完美绕开了校验引擎的崩溃 BUG。
- Windows 上的执行一次通过率和顺滑程度终于实现了等同于 macOS/Linux 的极限丝滑！

---

## 🛠️ 如何安装与使用

由于我们解除了原版中仅用于防篡改的 npm `prepare` 安装锁，你不需要任何复杂配置，使用 Node 环境直接即可安装。

1. **环境准备**
   确保你安装了 **[Node.js 18.0+](https://nodejs.org/)**

2. **全局安装 (二选一)**
   ```bash
   # 方法 A: 将本项目文件直接软链接至全局 (推荐给开发者)
   npm link

   # 方法 B: 全局安装 
   npm install -g .
   ```

3. **执行与唤醒**
   为了避免同你可能已经安装的官方源 `claude` 发生覆盖冲突，Neo Code 专属唤醒命令重定向为：
   ```bash
   neo
   ```
   在终端敲下 `neo`，开始进入属于 Windows 的次时代代码代工体验吧！

---

## 📜 版权申明与开源协议

本项目（含衍生代码劫持补丁 `patch-cli.mjs` 等改造部分）基于最宽松的 **MIT License** 开源，详见仓库下的 `LICENSE` 文件。

> **⚠️ 注意：** 
> 本项目专为兼容性研究与技术填补而设立。所有原版 `cli.js` 中包含的代理逻辑、大语言模型处理、MCP实现等**底层核心技术及源二进制体的 IP 权利完全归属 [Anthropic PBC] 所有**。
> 请同时遵守官方服务协议与许可条款：详见 [Anthropic Legal and Compliance](https://code.claude.com/docs/en/legal-and-compliance)。
