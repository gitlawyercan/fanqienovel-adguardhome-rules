# 番茄免费小说（红果免费短剧、红果免费漫剧） AdGuard Home 拦截规则

> 基于真实流量分析，精准拦截番茄免费小说（com.dragon.read）的埋点、广告、设备指纹等非必要请求，保留正常阅读、听书、短剧功能。
> 数据包来源说明：
> 1、使用GitHub上的fqnovel-adrules项目中的规则导入至Adguard安卓端；
> 2、笔者在使用规程中发现能够拦截番茄、红果系的广告，然后长时间使用app后导出事件数据，整理总结而来。
> Adguard安卓端所使用拦截广告项目：
> 名称：fqnovel-adrules
> 地址：https://github.com/changzhaoCZ/fqnovel-adrules
> 注意：请手机端直接使用fqnovel-adrules

## 📊 拦截效果

基于 10,000 条事件（约 41 分钟采样）的实测数据：

| 指标 | 数值 |
|------|------|
| 拦截总数 | **8,122 次** |
| DNS 层拦截 | 4,756 次（20 个域名） |
| QUIC 协议拦截 | 130 次 |
| 白名单放行 | 24 次 |

### 按用途分类

| 类别 | 拦截次数 | 占比 | 影响 |
|------|---------|------|------|
| 📊 埋点 / 日志上报 | 7,009 | 87.7% | 无影响，减少隐私泄露 |
| 🔒 设备指纹 / 风控 | 400 | 5.0% | 无影响，降低追踪 |
| 📢 广告投放 | 108 | 1.4% | 去除广告 |
| 🖼️ 图片 CDN | 138 | 1.7% | 已加入白名单 |
| 💬 IM / 推送 | 326 | 4.1% | 默认放行，可选拦截 |
| 📈 统计 SDK | 10 | 0.1% | 无影响 |

## 🚀 快速开始

### 方式一：通过 AdGuard Home Web 界面添加

1. 打开 AdGuard Home 管理页面
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

## 📋 规则说明

### 拦截规则

| 规则 | 说明 | 拦截次数 |
|------|------|---------|
| `\|\|*-applog*.fqnovel.com^` | fqnovel 埋点上报 | 3,908 |
| `\|\|mon11-misc-lq.fqnovel.com^` | 监控埋点 | 352 |
| `\|\|mssdk*.zijieapi.com^` | 设备指纹 SDK | 148 |
| `\|\|tnc3-alisc1.bytedance.com^` | 字节风控 | 124 |
| `\|\|ads*-normal*.zijieapi.com^` | 广告接口 | 88 |
| `\|\|tnc*.zijieapi.com^` | 风控接口 | 76 |
| `\|\|dig.bdurl.net^` | 百度埋点 | 2,665 |

### 白名单规则

| 规则 | 说明 | 必要性 |
|------|------|--------|
| `@@\|\|*-fq-tts.fqnovelvod.com^` | 听书 TTS 语音 | ⭐ 必须 |
| `@@\|\|v*-reading-video.fqnovelvod.com^` | 短剧/阅读视频 | ⭐ 必须 |
| `@@\|\|api*-normal.fqnovel.com^` | 核心 API | ⭐ 必须 |
| `@@\|\|*-reading.fqnovelstatic.com^` | 静态资源 | ⭐ 必须 |
| `@@\|\|p*-reading-private.fqnovel.com^` | 阅读器图片 | ⭐ 必须 |
| `@@\|\|tnc*-*.zijieapi.com^` | 评论区 | 可选 |
| `@@\|\|security.snssdk.com^` | 抖音登录 | 可选 |

## ⚠️ 注意事项

### 1. 首次加载可能较慢

由于拦截了大量埋点请求，首次打开 App 时可能会感觉加载稍慢。这是因为 App 会尝试连接被拦截的服务器，超时后才回落到正常接口。**使用 2-3 次后会恢复正常。**

### 2. 图片 CDN 处理

本规则对图片 CDN（byteimg.com / douyinpic.com）进行了白名单放行，确保书籍封面、推荐图、漫画图片正常显示。如果仍然遇到图片加载问题，可以在白名单中添加对应域名。

### 3. QUIC 协议

AdGuard Home 默认会阻断 QUIC/HTTP3 流量（UDP 443 端口），使请求回落到普通 HTTPS。如果需要完全阻断 QUIC，可以在路由器防火墙添加规则：

```bash
# OpenWrt（LuCI：网络 → 防火墙 → 通信规则 → 添加）
iptables -I FORWARD -p udp --dport 443 -j DROP
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

[MIT License](LICENSE)
