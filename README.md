# あおい解凍 AoiUnzip 🩵

网页版解压缩小屋 —— 打开网页就能解压，支持**带密码的加密压缩包**。所有解压都在你的浏览器本地完成，文件和密码**不会上传到任何服务器**。

![mascot](assets/mascot.webp)

## ✨ 功能

- 📦 支持格式：ZIP、7Z、RAR（v4/v5）、TAR、TAR.GZ / TGZ、TAR.BZ2、TAR.XZ、GZ、BZ2、XZ、ISO、CAB、CPIO、DEB 等
- 🔐 支持密码解压：
  - 加密 ZIP（AES-256 / ZipCrypto）
  - 加密 7Z（AES-256，含文件名加密）
  - 加密 RAR 等（libarchive 支持的格式）
- ⬇️ 解压后可逐个下载，也可以一键**全部打包成 ZIP 下载**
- 🧊 蓝色液态玻璃（Liquid Glass）+ 日系动漫风界面
- 🔒 纯静态页面，无后端、无上传、无追踪

## 🚀 使用

直接用浏览器打开 `index.html` 即可（需要通过 http(s) 访问，推荐 GitHub Pages 或任意静态服务器；直接双击以 `file://` 打开时 WebAssembly/worker 可能被浏览器拦截）。

启用 GitHub Pages：仓库 **Settings → Pages → Source 选 `main` 分支**，稍等片刻即可通过 `https://<用户名>.github.io/aoi-unzip/` 在线使用。

本地预览：

```bash
cd aoi-unzip
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

## 🛠️ 技术

双引擎，均为 WebAssembly，纯前端运行：

| 引擎 | 负责格式 |
| --- | --- |
| [libarchive.js](https://github.com/nika-begiashvili/libarchivejs)（libarchive 的 WASM 移植） | ZIP / RAR / TAR / GZ / BZ2 / XZ / ISO 等 |
| [7z-wasm](https://www.npmjs.com/package/7z-wasm)（7-Zip 24.09 的 WASM 移植） | 7Z（含 AES-256 加密、文件名加密） |
| [JSZip](https://stuk.github.io/jszip/) | “全部打包下载”时重新打包为 ZIP |

> 为什么 7Z 要单独一个引擎？libarchive.js 的构建对 7Z 加密内容会直接报“不支持”，实测后改为由真正的 7-Zip 引擎处理 .7z，加密 7Z 才能正常用密码解开。

## 📁 结构

```
.
├── index.html          # 页面 + 样式 + 逻辑（单文件）
├── assets/mascot.webp  # 吉祥物插画
└── lib/                # 引擎文件（本地内置，不依赖 CDN）
    ├── libarchive.js / worker-bundle.js / libarchive.wasm
    ├── 7zz.js / 7zz.wasm
    └── jszip.min.js
```

## ⚠️ 小提示

- 解压在浏览器内存中进行，特别大的压缩包（数百 MB 以上）在手机上可能较慢或内存不足。
- 密码忘了是找不回来的，本工具不做任何密码破解/爆破。
