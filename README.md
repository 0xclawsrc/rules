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
  # 把 PROXY 替换为你的策略组名称，例如「代理」或「自动选择」
  - RULE-SET,LongBridge,PROXY
  - RULE-SET,Claude,PROXY
```

### 备用 CDN 地址（jsDelivr，部分网络下更快/更稳）

```
https://cdn.jsdelivr.net/gh/0xclawsrc/rules@main/ruleset/LongBridge.list
https://cdn.jsdelivr.net/gh/0xclawsrc/rules@main/ruleset/Claude.list
```

## 说明

- 文件为 **classical（通用）** 行为的规则集：每行只写规则、不带策略，策略在主配置 `rules:` 的 `RULE-SET,<名称>,<策略组>` 中指定。
- `RULE-SET` 规则按书写顺序匹配，建议放在 `MATCH`/`FINAL` 兜底规则之前。
- 这些规则只负责「分流到哪个策略组」，能否正常访问取决于对应节点的地区与可用性（Claude 对地区较敏感）。
