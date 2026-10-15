# Shadowrocket 防 DNS / WebRTC 泄漏配置

一份面向 Shadowrocket（iOS）的通用配置，目标是让 DNS 查询和 WebRTC 都不暴露真实 IP。配置中不含任何节点、密码或订阅信息，节点请在 Shadowrocket 中自行添加。

## 功能

- 全部流量由 TUN 接管，局域网地址直连，其余全部走代理。
- DNS 只使用加密的 DoH（Cloudflare、Google），禁止回落到系统或运营商 DNS。
- 关闭本机 IPv6 出口，避免 IPv6 绕过代理。
- 拦截 WebRTC 常用的 STUN/TURN 端口（3478、3479、5349、19302）和 STUN 域名。
- 拦截 UDP 443（QUIC），强制浏览器流量走 TCP 代理。
- 节点不支持 UDP 时直接拒绝 UDP，不会绕过代理直连。

## 使用方法

1. 在 Shadowrocket 中进入「配置」，点右上角的 `+`，粘贴本文件的 Raw 链接并下载。
2. 点击下载好的配置，选择「使用配置」。
3. 回到首页，把「全局路由」设为「配置」，选择你的节点并打开开关。
4. 建议使用支持 UDP 转发的节点，加密方式选 AEAD（如 `aes-256-gcm`、`chacha20-ietf-poly1305`、`2022-blake3-*`）。

## 验证

连接后打开以下网站，看到的只应该是节点的 IP 和境外 DNS：

- https://ipleak.net
- https://browserleaks.com/webrtc
- https://www.dnsleaktest.com

如果检测页显示 IPv6 地址，但地区和运营商与节点一致，那是节点服务器的出口地址，不是泄漏。

## 可选调整

- 需要 UDP 游戏或语音通话、且节点支持 UDP 时，可以删除拦截 UDP 443 的那条规则。STUN 端口的拦截规则请保留。
- 想完全禁止 IPv6 访问，可以在 `[Rule]` 最前面加一行 `IP-CIDR6,::/0,REJECT`。
- 部分键名在不同版本的 Shadowrocket 中可能不被支持，导入报错时删除对应行即可。

## 注意

- 浏览器内部也可能产生 WebRTC 泄漏。桌面端建议安装 WebRTC 防泄漏扩展。
- 请勿把含节点信息或订阅链接的配置提交到公开仓库。

## 免责声明

本配置仅供学习和保护个人隐私使用，请遵守当地法律法规。
