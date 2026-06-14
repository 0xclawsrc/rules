# rules

自用分流规则集（classical / 规则集 rule-provider 格式），适用于 **Stash / Clash.Meta / Surge / Shadowrocket / Quantumult X**。

## 规则列表

| 名称 | 文件 | 说明 |
| --- | --- | --- |
| 长桥证券 Longbridge | [`ruleset/LongBridge.list`](./ruleset/LongBridge.list) | 长桥/LongPort 主站、APP、OpenAPI、行情、CDN、PortAI |
| Claude / Anthropic | [`ruleset/Claude.list`](./ruleset/Claude.list) | anthropic.com、claude.ai、claude.com、Artifacts 等 |

## 在 Stash 中使用

Stash 兼容 Clash 配置。在你的配置文件中加入下面的 `rule-providers` 与 `rules` 片段即可（远程订阅，自动每天更新一次）。

> 默认分支按 `main` 填写，如你的默认分支不同，请把 URL 里的 `main` 替换为对应分支名。

```yaml
rule-providers:
  LongBridge:
    type: http
    behavior: classical
    format: text
    url: "https://raw.githubusercontent.com/0xclawsrc/rules/main/ruleset/LongBridge.list"
    interval: 86400
    path: ./ruleset/LongBridge.list

  Claude:
    type: http
    behavior: classical
    format: text
    url: "https://raw.githubusercontent.com/0xclawsrc/rules/main/ruleset/Claude.list"
    interval: 86400
    path: ./ruleset/Claude.list

rules:
  # 🚀 节点选择 需与你 proxy-groups 中已有的组名一致
  - RULE-SET,LongBridge,🚀 节点选择
  - RULE-SET,Claude,🚀 节点选择
```

### 备用 CDN 地址（jsDelivr，部分网络下更快/更稳）

```
https://cdn.jsdelivr.net/gh/0xclawsrc/rules@main/ruleset/LongBridge.list
https://cdn.jsdelivr.net/gh/0xclawsrc/rules@main/ruleset/Claude.list
```

## 覆写文件（Override，从 URL 添加）

若你的 Stash 用「从 URL 添加覆写」，可直接订阅下面现成的覆写文件（已内置 `rule-providers` 引用 + `RULE-SET`，分流到 `🚀 节点选择`）：

| 覆写 | 内容 | URL |
| --- | --- | --- |
| 仅长桥 | LongBridge | `https://raw.githubusercontent.com/0xclawsrc/rules/main/override/LongBridge.yaml` |
| 长桥 + Claude | LongBridge + Claude | `https://raw.githubusercontent.com/0xclawsrc/rules/main/override/LongBridge-Claude.yaml` |

两个覆写都带 `name`/`desc`（导入后 Stash 里能看到名称与描述），并自带独立策略组：

- `override/LongBridge.yaml` → 策略组「**长桥 Longbridge**」
- `override/LongBridge-Claude.yaml` → 策略组「**长桥 Longbridge**」「**Claude AI**」

> 这些策略组默认跟随你的主策略组 `🚀 节点选择`，导入后可在 Stash 里对长桥/Claude **单独切换**节点。如果你的主策略组不叫 `🚀 节点选择`，改文件里 `proxy-groups` 的 `proxies` 即可。

## 说明

- 文件为 **classical（通用）** 行为的规则集：每行只写规则、不带策略，策略在主配置 `rules:` 的 `RULE-SET,<名称>,<策略组>` 中指定。
- `RULE-SET` 规则按书写顺序匹配，建议放在 `MATCH`/`FINAL` 兜底规则之前。
- 这些规则只负责「分流到哪个策略组」，能否正常访问取决于对应节点的地区与可用性（Claude 对地区较敏感）。
