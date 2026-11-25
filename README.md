# Personal RoutingA rules

自动汇总长期维护的公开名单，生成 v2rayA RoutingA 文本。每日北京时间 08:23 由 GitHub Actions 构建，也可手动运行。GitHub 定时任务可能延迟，公共仓库长期无活动时定时任务可能被 GitHub 暂停。

## 订阅链接（点击复制）

每个地址单独放在代码块内，点击代码块右上角的复制按钮即可复制完整链接。这些是 RoutingA 规则文件地址，不是代理节点订阅。

### 黑名单

已知国外走代理、已知广告拦截、其他直连。[点击打开规则](https://raw.githubusercontent.com/dqipb200706/routinga-rules/main/dist/routingA-blacklist.txt)

```text
https://raw.githubusercontent.com/dqipb200706/routinga-rules/main/dist/routingA-blacklist.txt
```

### 白名单

已知国内走直连、已知广告拦截、其他代理。[点击打开规则](https://raw.githubusercontent.com/dqipb200706/routinga-rules/main/dist/routingA-whitelist.txt)

```text
https://raw.githubusercontent.com/dqipb200706/routinga-rules/main/dist/routingA-whitelist.txt
```

原地址 `routingA.txt` 继续提供黑名单内容，避免原链接失效：

```text
https://raw.githubusercontent.com/dqipb200706/routinga-rules/main/dist/routingA.txt
```

[来源版本、条数及两份文件校验](dist/manifest.json)

这不是节点订阅。v2rayA 的节点订阅框不能导入此文件；可将内容导入 RoutingA 编辑器。自动下载并应用需要单独的客户端更新工具，本仓库不会连接或修改树莓派，当前没有部署自动应用脚本。

## 策略与优先级

**黑名单**：个人例外优先 → 上游广告拦截 → 已知国外域名代理 → 其余直连。未被代理名单收录的境外网站可能无法直连，需要添加代理例外。

**白名单**：个人例外优先 → 上游广告拦截 → 已知国内域名及中国 IPv4/IPv6 网段直连 → 其余代理。未被国内名单收录的国内网站可能误走代理，需要添加直连例外。

两套规则均让局域网 IP/私有域名直连；广告与非广告域名都按从上到下的首条匹配规则生效。个人例外中的直连、代理、拦截项对两套规则都有效。

每套规则内，上游广告优先于该套名单。父域和子域交叉时同样按顺序生效（例如广告父域可能覆盖后面的直连子域）；用个人直连例外放行。规则中不使用宽泛的关键词匹配；保留 DOMAIN-SET 的准确域名与后缀语义。代理名单不是全球全部国外域名。广告只能按域名拦截，不能清除同域视频广告或页面元素。

## 来源与许可

- [Loyalsoldier/surge-rules](https://github.com/Loyalsoldier/surge-rules)：release 分支 direct/proxy/reject/private.txt，域名集来源见该项目 README；遵守其 [GPL-3.0 许可](https://github.com/Loyalsoldier/surge-rules/blob/master/LICENSE) 及上游声明。
- [PaPerseller/chn-iplist](https://github.com/PaPerseller/chn-iplist)：master 分支中国 IPv4/IPv6 CIDR；[MIT 许可](https://github.com/PaPerseller/chn-iplist/blob/master/LICENSE)。
- 生成器源代码采用 GPL-3.0-or-later。上游数据保留各自许可；并不主张对上游数据重新授权。上游许可原文放在 licenses/。

每次固定上游 commit 后下载同一快照，manifest 记录地址、commit、SHA256 和条数。中国 IP 直接内嵌到输出，避免引用 geoip:cn 却没有实际使用指定上游数据。局域网仍使用核心内置 geoip:private。

## 个人例外

编辑 custom/direct.txt、proxy.txt、reject.txt：一行一个域名；example.com 仅精确匹配，.example.com 包含该域及子域。以 # 开头为注释。不要把内网秘密地址、节点凭据或 Token 提交到这个公共仓库。

## 构建与失败处理

Python 3.12+，仅标准库：

```sh
python3 scripts/build.py
```

下载、格式、最低条数校验全部通过后才替换输出。相对上次条数减少超过 30% 或增加超过 50% 时中止；发布失败保留上次已提交版本，检查 Actions 日志后再决定是否接受变化。个人三类例外完全相同的匹配项冲突会报错。实际 v2rayA 核心兼容性与目标设备负载仍需部署前验证；本仓库构建成功不代表树莓派已应用或已完成端到端分流验收。
