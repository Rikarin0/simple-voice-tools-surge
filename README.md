# Simple Voice Tool for Surge

本仓库提供已构建的 Surge 脚本、模块和完整安装包，入口为 **https://voice.tool/**。下载 [完整安装包](Simple-Voice-Tool-Surge.zip) 后按以下步骤安装。

此版本由 Surge 的 `http-request` 脚本在本机提供完整网页，用 Safari / Chrome 运行录音、分析、历史记录和设置。网页功能与站点版使用同一份构建产物，麦克风由浏览器授权。无需 Node.js、服务器或 CDN；可选 GitHub 云备份仍需联网。

## 安装

1. 解压 `Simple-Voice-Tool-Surge.zip`。把 `Simple-Voice-Tool.js` 和 `Simple-Voice-Tool.sgmodule` 放入 **当前 Surge 配置文件所在目录**。脚本的相对路径以配置文件目录为准，不是模块下载目录。也可在 Surge 脚本编辑器导入 `.js`，再将模块的 `script-path` 改为其实际路径。
2. 在 Surge 的模块列表中启用 `Simple Voice Tool` 本地模块。使用托管配置时，可导入模块；务必确认该模块指向已导入的脚本文件。
3. 在 Surge 中启用脚本、MITM（HTTPS 解密），生成并安装本机 CA 证书。iOS 还需在“设置 → 通用 → 关于本机 → 证书信任设置”中开启该证书的完全信任；macOS 在钥匙串中信任该证书。模块仅追加 `voice.tool`，证书由你自己的 Surge 配置管理。
4. 启动 Surge。iOS 启用 Surge VPN；macOS 使用 Surge 系统代理或增强模式，使浏览器请求经过 Surge。用 **Safari / Chrome** 打开 **https://voice.tool/**，允许麦克风访问。不要在 Surge 脚本编辑器中运行录音页面。

模块内的 `[Host]` 映射用于本地解析；匹配的请求直接返回打包资源，不连接该 IP。此入口由本机 Surge 提供，请同时启用脚本和模块。若已有更优先匹配同一域名的脚本，调整模块顺序，因为每个请求仅执行首个匹配的 `http-request` 脚本。由旧域名版本更新时，请替换原来的脚本与模块，并重新加载配置。

## 数据、离线与更新

- 默认入口为 `https://voice.tool/`，它与原站点使用独立的 localStorage、IndexedDB 和麦克风权限。首次访问新域名时，需要重新允许麦克风访问。
- 换设备、浏览器、浏览器配置文件或域名时，在旧页面“设置 → 完整备份”导出 ZIP，再在新页面导入。模块和安装包均不包含个人录音、历史数据、令牌或证书。
- 音高/共振峰/能量分析、三种测试模式、回放、历史/趋势/对比、图表、主题与四种语言、CSV/JSON/PNG 导出、ZIP 备份恢复、实验性功能以及 PWA 保留。File System Access 等原有可选能力继续受浏览器支持情况影响。
- 完整页面、Service Worker、manifest 和图标已打包。首次加载时保持 Surge 运行；浏览器完成 PWA 缓存后可使用原有离线功能。iOS 可通过 Safari 分享菜单“添加到主屏幕”。
- 更新时同时替换 `.js` 与 `.sgmodule`，并在 Surge 中重新加载脚本/配置。再刷新页面以更新 PWA；若已有缓存暂时显示旧版本，关闭所有应用窗口后重新打开。**不要清除网站数据来更新**，以免删除本地记录。

## 安装包内容

| 文件 | 用途 |
| --- | --- |
| `Simple-Voice-Tool.sgmodule` | Surge 模块配置 |
| `Simple-Voice-Tool.js` | 包含完整网页资源的本地响应脚本 |
| 安装包内 `web/` | 与网页构建产物逐字节相同的静态文件，供检查或单独部署 |
| `SHA256SUMS.json` | 网页资源及安装文件的 SHA-256 校验值 |
| `THIRD_PARTY_LICENSES.txt` | 脚本解压组件许可证 |

## 验证范围

构建校验在隔离的 JavaScript 环境中执行实际生成的 Surge 脚本，逐一检查全部资源的字节、MIME、查询参数、HTTP→HTTPS 跳转、HEAD、缓存条件、错误路由和安装包校验值，并检查 PWA 资源完整性。此环境没有 Surge 客户端和实体麦克风，TLS 解密、证书信任与真机录音需在安装后验证。

协议依据：[HTTP Request Script](https://manual.nssurge.com/scripting/http-request.html)、[Scripting](https://manual.nssurge.com/scripting/overview.html)、[Module](https://manual.nssurge.com/profile/module.html)、[Local DNS Mapping](https://manual.nssurge.com/dns/local-dns-mapping.html)、[HTTPS Decryption](https://manual.nssurge.com/http/mitm.html)。
