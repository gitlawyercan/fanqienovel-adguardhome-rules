# 番茄免费小说 AdGuard Home 拦截规则

> 基于真实流量分析，精准拦截番茄免费小说（com.dragon.read）和红果免费短剧（com.phoenix.read）的埋点、广告、设备指纹等非必要请求，保留正常阅读、听书、短剧功能。

## 📖 数据来源

> **数据包来源说明：**
>
> 1. 使用 GitHub 上的 [fqnovel-adrules](https://github.com/changzhaoCZ/fqnovel-adrules) 项目中的规则导入至 AdGuard 安卓端；
> 2. 笔者在使用过程中发现能够拦截番茄、红果系的广告，然后长时间使用 App 后导出事件数据，整理总结而来。

### AdGuard 安卓端所使用拦截广告项目

| 项目 | 说明 |
|------|------|
| **名称** | fqnovel-adrules |
| **地址** | https://github.com/changzhaoCZ/fqnovel-adrules |
| **适用平台** | Android（AdGuard App） |

> ⚠️ **注意：手机端请直接使用 [fqnovel-adrules](https://github.com/changzhaoCZ/fqnovel-adrules)**，本项目（AdGuard Home 规则）基于其流量分析数据转换而来，适用于路由器/家庭网络的 DNS 级过滤。

## 📊 拦截效果

基于 50,000 条事件（5 批次数据，覆盖番茄小说 + 红果短剧）的实测数据：

| 指标 | 数值 |
|------|------|
| 总事件数 | **50,000 条** |
| DNS 查询 | 27,359 次 |
| 隧道请求 | 21,296 次 |
| QUIC 拦截 | 1,288 次 |
| 独立域名 | 905 个 |

### 按用途分类

| 类别 | 域名数 | 拦截优先级 | 说明 |
|------|--------|-----------|------|
| 📊 埋点 / 日志上报 | ~20 | ⭐ 高 | 减少隐私泄露 |
| 🔒 设备指纹 / 风控 | ~15 | ⭐ 高 | 降低追踪 |
| 📢 广告投放 | ~26 | ⭐ 高 | 去除广告 |
| 📈 统计 SDK | ~10 | ⭐ 高 | 减少数据收集 |
| 🖼️ 图片 CDN | ~23 | 🟡 中 | 部分含广告图片 |
| 💬 IM / 推送 | ~14 | 🟡 中 | 可能影响推送 |
| 🎮 电商 / 小游戏 | ~7 | 🟡 中 | 去除电商推广 |
| 📹 直播 CDN | 200+ | ⚪ 低 | 看直播需放行 |

## ⚡ 重要：Certificate Pinning（UNKNOWN_CA）

### 什么是 UNKNOWN_CA？

部分域名使用了 **Certificate Pinning（证书固定）**，只信任字节跳动自己的 CA 证书。AdGuard Home 做 HTTPS 中间人解密时，App 发现不认识 AdGuard 的 CA 证书，直接拒绝连接。

### 受影响的广告域名（DNS 拦截有效）

这些域名**是广告来源**，但拒绝 AdGuard 的 HTTPS 证书：

| 域名 | 次数 | 类型 |
|------|------|------|
| `ec*-core-lq.ecombdapi.com` | 86 | 电商广告核心 |
| `ecom*-normal-lq.ecombdapi.com` | 26 | 电商广告 |
| `isaas*-normal-lq.ecombdapi.com` | 9 | 电商广告 |
| `ads*-normal-lq.zijieapi.com` | 21 | 广告投放 |
| `gecko*-*.zijieapi.com` | 28 | 电商 SDK |
| `www.chengzijianzhan.com` | 7 | 广告追踪 |
| `praisewindow-sinfonlinea.ugsdk.cn` | 6 | 广告窗口 |
| `p*-ad-sign.byteimg.com` | 3 | 广告签名图 |
| `p*-developer-sign.bytemaimg.com` | 4 | 开发者广告 |

**结论：这些域名的 DNS 拦截有效，HTTPS 过滤无效。不要将它们加入 HTTPS 排除列表！**

### 受影响的功能域名（需白名单放行）

| 域名 | 次数 | 类型 |
|------|------|------|
| `api*-normal*.fqnovel.com` | 82 | ⭐ 核心 API |
| `frontier*-toutiao-lq.fqnovel.com` | 25 | ⭐ 推荐流 |
| `lf-fe.fqnovelstatic.com` | 5 | ⭐ 静态资源 |
| `p*-reading-sign.*.com` | 27 | ⭐ 阅读器图片 |
| `p*-novel*.byteimg.com` | 141 | 漫画/出版图片 |
| `*.douyinpic.com` | 116 | 图片 CDN |

## 🚀 快速开始

### 方式一：通过 AdGuard Home Web 界面添加

1. 打开 AdGuard Home 管理页面（默认 `http://192.168.1.1:3000`）
2. 进入 **过滤 → DNS 拦截**
3. 点击 **添加拦截规则**
4. 粘贴 `fanqienovel.txt` 中 **拦截规则** 部分（`! =====` 分隔线之间的内容）
5. 进入 **过滤 → DNS 白名单**
6. 粘贴 `fanqienovel.txt` 中 **白名单** 部分

### 方式二：远程订阅（推荐）

将 `fanqienovel.txt` 托管后，在 AdGuard Home 的 **过滤 → DNS 拦截 → 添加拦截规则列表** 中添加：

```
https://raw.githubusercontent.com/gitlawyercan/fanqienovel-adguardhome-rules/main/fanqienovel.txt
```

### HTTPS 过滤排除列表（推荐）

由于部分域名使用 Certificate Pinning，建议在 AdGuard Home 的 **设置 → HTTPS 进过滤 → 排除域名** 中添加以下功能域名：

```
api*-normal*.fqnovel.com
frontier*-toutiao-lq.fqnovel.com
lf-fe.fqnovelstatic.com
lf3-reading.fqnovelpic.com
*.snssdk.com
*.douyin.com
```

> ⚠️ **不要排除广告域名**（ecombdapi.com、ads*.zijieapi.com 等），让 DNS 拦截生效。

## 📋 规则说明

### 拦截规则（按优先级）

| 规则 | 说明 | UNKNOWN_CA |
|------|------|-----------|
| `\|\|*-applog*.fqnovel.com^` | fqnovel 埋点上报 | ❌ |
| `\|\|dig.bdurl.net^` | 百度埋点 | ⚠️ 有 |
| `\|\|mssdk*.zijieapi.com^` | 设备指纹 SDK | ❌ |
| `\|\|tnc*.zijieapi.com^` | 风控接口 | ❌ |
| `\|\|ads*-normal*.zijieapi.com^` | 广告投放 | ⚠️ 有 |
| `\|\|ecom*-normal-lq.ecombdapi.com^` | 电商广告 | ⚠️ 有 |
| `\|\|ec*-core-lq.ecombdapi.com^` | 电商广告核心 | ⚠️ 有 |
| `\|\|gecko*-*.zijieapi.com^` | 电商 SDK | ❌ |
| `\|\|pull-*.douyincdn.com^` | 直播 CDN | 可选拦截 |

### 白名单规则

| 规则 | 说明 | UNKNOWN_CA | 必要性 |
|------|------|-----------|--------|
| `@@\|\|*-fq-tts.fqnovelvod.com^` | 听书 TTS 语音 | ❌ | ⭐ 必须 |
| `@@\|\|v*-reading-video*.qznovelvod.com^` | 短剧/阅读视频 | ❌ | ⭐ 必须 |
| `@@\|\|api*-normal.fqnovel.com^` | 核心 API | ⚠️ 有 | ⭐ 必须 |
| `@@\|\|lf-fe.fqnovelstatic.com^` | 静态资源 | ⚠️ 有 | ⭐ 必须 |
| `@@\|\|p*-reading-sign.*.com^` | 阅读器图片 | ⚠️ 有 | ⭐ 必须 |
| `@@\|\|p*-novel*.byteimg.com^` | 漫画图片 | ⚠️ 有 | ⭐ 必须 |
| `@@\|\|*.douyinpic.com^` | 图片 CDN | ⚠️ 有 | ⭐ 必须 |
| `@@\|\|tnc*-*.zijieapi.com^` | 评论区 | ❌ | 可选 |
| `@@\|\|security.snssdk.com^` | 抖音登录 | ❌ | 可选 |
| `@@\|\|pull-*.douyincdn.com^` | 直播流 | ❌ | 可选 |

## ⚠️ 注意事项

### 1. 首次加载可能较慢

由于拦截了大量埋点请求，首次打开 App 时可能会感觉加载稍慢。这是因为 App 会尝试连接被拦截的服务器，超时后才回落到正常接口。**使用 2-3 次后会恢复正常。**

### 2. 图片 CDN 处理

本规则对图片 CDN（byteimg.com / douyinpic.com）进行了白名单放行，确保书籍封面、推荐图、漫画图片正常显示。这些域名使用了 Certificate Pinning，HTTPS 过滤无效，DNS 拦截也无效（已加入白名单）。

### 3. QUIC 协议

AdGuard Home 默认会阻断 QUIC/HTTP3 流量（UDP 443 端口），使请求回落到普通 HTTPS。如果需要完全阻断 QUIC，可以在路由器防火墙添加规则：

```bash
# OpenWrt（LuCI：网络 → 防火墙 → 通信规则 → 添加）
iptables -I FORWARD -p udp --dport 443 -j DROP
```

### 4. UNKNOWN_CA 错误

如果在 AdGuard Home 日志中看到大量 `UNKNOWN_CA` 错误，这是正常现象。这些域名使用了 Certificate Pinning，AdGuard 的 HTTPS 过滤无法解密。解决方案：

- **广告域名**：DNS 拦截已足够，不需要 HTTPS 过滤
- **功能域名**：已加入白名单放行
- **如需完全解密**：需 Root 后将 AdGuard CA 安装为系统级证书

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

[MIT License](LICENSE)
