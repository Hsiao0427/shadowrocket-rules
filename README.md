# shadowrocket-rules

本仓库维护 Shadowrocket 和 Clash Verge（Mihomo）使用的域名规则清单。

| 文件 | `ruleset-id` | 服务范围 |
| --- | --- | --- |
| [SpaceX.list](SpaceX.list) | `spacex` | Cursor、Grok（xAI）、X（Twitter）及其相关登录服务 |
| [PlayStation.list](PlayStation.list) | `playstation` | PSN、商店、游戏下载和补丁、系统更新、PlayStation Plus、Remote Play、PlayStation Portal |

## 每日更新规则

本节适用于人工维护和定时任务。每天从 `main` 读取两份清单，根据各文件的注释头独立检查。提交由核实后的域名规则变动触发。

### 1. 读取文件头

| 注释字段 | 用途 |
| --- | --- |
| `schema-version` | 文件头格式版本，当前为 `1` |
| `ruleset-id`、`services` | 识别规则集及其覆盖的服务，`services` 是 JSON 数组 |
| `scope`、`excludes` | 确定可收录的用途和排除范围 |
| `managed-rule-types`、`format` | 确定可维护的规则类型和文件格式 |
| `update-policy`、`source` | 确认更新约定和参考来源，每条 `source` 单独一行 |

### 2. 核实域名增删

- **查证来源。** 从文件的 `source` 开始，优先使用官方文档、公告和官方服务页面。第三方规则集用于发现候选域名，收录前再核实它与对应服务的关系及用途。
- **新增。** 确认域名的用途符合 `scope`，并检查 `excludes`。单一主机用 `DOMAIN`。只有确认整个后缀都适用时才用 `DOMAIN-SUFFIX`。共享 CDN 或登录服务如有可核实的专用主机，只收录该主机。
- **移除或替换。** 有明确证据表明域名已停用、被替换、转手或超出维护范围时才调整。短时访问失败、一次 DNS 查询失败、第三方规则集删除或当天来源不可用时，保留已有规则。
- **记录依据。** 每个拟变更项记录文件名、域名、增删原因和证据链接。有疑问的域名留待下次核实。

### 3. 校验文件并提交

1. 只编辑 `DOMAIN` 和 `DOMAIN-SUFFIX` 规则行。保留文件头、分组注释、未变动规则的顺序和所有非域名规则，包括 `PlayStation.list` 的 `IP-ASN` 条目。
2. 每行写一条规则，格式为 `DOMAIN,主机名` 或 `DOMAIN-SUFFIX,域名后缀`。域名用小写，去掉协议、端口、路径、通配符和末尾的点。检查重复项，以及新规则是否已被同文件的后缀规则覆盖。
3. 提交前对照 `main` 检查差异，确认只有经过核实的域名规则增删。若检查期间 `main` 有新提交，先读取最新文件再重新核对。
4. 有实际变动时提交受影响的清单，在提交说明中列出增删项及证据链接。无变动时只记录检查结果。遇到暂时无法读取的来源，在任务结果里注明。
