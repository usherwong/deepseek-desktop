# DeepSeek Desktop

**无需终端，网页里配置即用 · 粘贴图片直接看懂**

一个开源 DeepSeek 桌面客户端——不用敲命令，在网页设置界面里填 API key 就能用；粘贴图片即可自动识别。

> ⚠️ **非官方客户端，基于开源 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 构建。**

[📖 English](README.md)

<p align="center">
  <a href="https://github.com/usherwong/deepseek-desktop/releases/latest"><img alt="GitHub release" src="https://img.shields.io/github/v/release/usherwong/deepseek-desktop"></a>
  <a href="https://github.com/usherwong/deepseek-desktop/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/github/license/usherwong/deepseek-desktop"></a>
  <a href="https://github.com/usherwong/deepseek-desktop/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/usherwong/deepseek-desktop"></a>
</p>

<p align="center">
  <img src="docs/assets/vision.png" alt="识图效果" width="46%" />
  <img src="docs/assets/imagemodel.png" alt="DeepSeek (Image) 需要两个 key" width="46%" />
</p>

## 为什么用它

解决两个痛点：

1. **免终端** —— 不用敲命令、不用配环境变量，在「设置 → 模型」里填 key 就能用。
2. **粘贴图片即可识图** —— 把图粘进对话框，先由视觉模型看懂，再让 DeepSeek 回答你。

## 识图是怎么工作的

识图用的是**视觉模型**（`qwen3.7-plus`，来自 Qwen / 阿里百炼，也就是 qwen-mm-plugins 用的那套模型），跑在 **`DeepSeek (Image)`** 这个提供方下。

所以要用识图，你必须：

- 选 **`DeepSeek (Image)`** 这个模型，并且
- 在这个模型里填**两个 API key**：
  1. **DeepSeek API key**（给文本模型用），
  2. **百炼（DashScope）API key**（给视觉模型用）。

> 只用文字聊天，填 DeepSeek key 即可；要用识图，**两个 key 都要**。

## 特性

- ✅ 原生桌面应用（Electron），macOS + Windows
- ✅ 网页式配置：API key、模型、baseURL 全在 GUI 里填
- ✅ **粘贴图片即可识图**（截图、照片、含文字的图都能读）
- ✅ 内置 qwen-mm-plugins 媒体工具（OCR / 视觉问答 / 视频理解 / 语音转写）
- ✅ 完整 harness 能力：会话历史、子智能体、代码模式……

## 下载

> 最新版本 —— 见[全部 Releases](https://github.com/usherwong/deepseek-desktop/releases)

| 平台 | 架构 | 下载 |
|---|---|---|
| macOS（Apple Silicon） | arm64 | [.dmg](https://github.com/usherwong/deepseek-desktop/releases/latest/download/DeepSeek.Desktop-0.1.8-mac-arm64.dmg) · [.zip](https://github.com/usherwong/deepseek-desktop/releases/latest/download/DeepSeek.Desktop-0.1.8-mac-arm64.zip) |
| macOS（Intel） | x64 | [.dmg](https://github.com/usherwong/deepseek-desktop/releases/latest/download/DeepSeek.Desktop-0.1.8-mac-x64.dmg) · [.zip](https://github.com/usherwong/deepseek-desktop/releases/latest/download/DeepSeek.Desktop-0.1.8-mac-x64.zip) |
| Windows | x64 | [安装版 .exe](https://github.com/usherwong/deepseek-desktop/releases/latest/download/DeepSeek.Desktop-0.1.8-win-x64-setup.exe) · [绿色版 .exe](https://github.com/usherwong/deepseek-desktop/releases/latest/download/DeepSeek.Desktop-0.1.8-win-x64-portable.exe) |

> macOS 首次打开提示「无法验证开发者」时：**右键 App → 打开**，或执行
> `xattr -dr com.apple.quarantine "/Applications/DeepSeek Desktop.app"`。
> （正式公证需要 Apple Developer 账号，暂未提供。）

## 快速开始

1. 打开 App → **设置 → 模型**。
2. **DeepSeek**：填 DeepSeek API key。
3. **DeepSeek (Image)**：填 DeepSeek API key **和**百炼（DashScope）API key。
4. 新建会话 → 模型切到 **DeepSeek (Image)** → 粘贴图片。

## 从源码构建

```bash
# 1. 克隆本仓库（shell）
git clone https://github.com/usherwong/deepseek-desktop.git
cd deepseek-desktop

# 2. 克隆 harness（image-bridge 分支）
git clone https://github.com/usherwong/deepseek-harness.git harness
cd harness && git checkout image-bridge && cd ..

# 3. 安装 shell 依赖
npm ci

# 4. 从源码构建运行时 + 打包
node scripts/prepare-runtime.mjs --mode source --repo harness
npx electron-builder --mac --arm64   # 或 --mac --x64 / --win --x64
```

## 目录结构

```
.
├── src/            Electron 主进程 + preload
├── scripts/        运行时打包脚本（prepare-runtime.mjs）
├── build/          图标、签名、entitlements
├── harness.json    harness 构建配置（source 模式指向 harness/）
├── .github/        CI（自动出 mac arm64/x64 + Windows 安装包）
└── docs/           官网落地页 + 运营方案
```

## FAQ

**Q: 为什么识图要单独填一个百炼 key？**
DeepSeek 是纯文本模型，不会看图。App 用百炼视觉模型（`qwen3.7-plus`）先把图转成文字描述，再交给 DeepSeek。

**Q: 不填百炼 key 能用吗？**
能用，但只能文字聊天；粘贴图片会提示缺少视觉模型 key。

**Q: 这是官方产品吗？**
不是。第三方开源客户端，只调用**你自己的** API key。

## License

[MIT](./LICENSE) © usherwong

基于开源 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 构建，其版权归原作者所有。
