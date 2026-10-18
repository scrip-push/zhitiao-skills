# CLI 查询参考

基于 `@hrbully/zt-cli@0.1.0`。Node.js 24+；CLI 固定连接 https://offertt.cn 。用各级 `--help` 核实已安装版本的参数。下文 `ID` 必须替换为真实列表返回的 ID；枚举优先从发现命令获取。

## 招聘公告

```sh
zt recruitment filters --json
zt recruitment list --search-type job --search '软件工程师' --company-type '央国企' --sort deadline --json
zt recruitment list --search-type company_name --search '中国移动' --json
zt recruitment list --search-type title --search '校园招聘' --json
zt recruitment view ID --json
```

`--company-type`、`--location`、`--batch`、`--year` 接受一个或多个发现接口返回的有效值；示例“央国企”也应以当前发现结果为准。`--sort` 为 `latest` / `deadline` / `company`。省略 `--search-type` 时关键词按岗位检索。筛选返回整条公告，详情中的岗位信息不等于独立的可投递记录。

## 本人面经

```sh
zt interview list --search '开发' --json
zt interview list --stage business_interview --json
zt interview view ID --json
```

面试阶段：`written_test`、`business_interview`、`hr_interview`、`other`。

## 本人投递

```sh
zt application stages --json
zt application list --sort updated_desc --json
zt application list --stage STAGE --json
zt application list --custom-stage CUSTOM_STAGE_ID --json
zt application view ID --json
```

`STAGE` 和 `CUSTOM_STAGE_ID` 从 `stages` 输出获取；`--stage` 与 `--custom-stage` 互斥。排序支持 `created_desc`、`updated_desc`、`updated_asc`。默认排除不感兴趣/已排除，统计时声明这一口径。

## 分页与结构化输出

- 列表每页 20 条。`--page 2` 取下一页；`--all` 自动遍历，最多 100 页/2000 条；二者互斥。
- `--json` 和 `--jsonl` 互斥。JSON 包装含 `schema_version`（字符串 `"1"`）、`data`、`meta`、`error`。列表记录在 `data.items`，详情在 `data`。
- `meta.complete=true` 表示本次请求完成；单页成功不代表拿到了全量。结合 `meta.next_page`、`meta.total`、`meta.pages` 等实际返回字段判断范围，不将去重后的已取条数当成全站总数。
- 读取命令（包括 `auth status`、`recruitment filters`、`application stages`）支持 `--timeout 1..600` 秒。`auth login` 不支持 `--json` 或 `--timeout`；`auth logout` 和 `doctor` 只支持 `--json` 等各自帮助中列出的选项，不支持 `--timeout`。
- JSONL 逐条输出，最后一条为 result；必须检查尾记录和进程退出码，缺少尾记录不能认定完成。

## 失败与恢复

| 退出码 | 含义 | 处理 |
| --- | --- | --- |
| 0 | 成功 | 仍检查 error 与完成范围 |
| 1 | 内部、契约、存储或锁错误 | 保留错误信息，可用 `zt doctor --json` 诊断；不自行删除凭证 |
| 2 | 参数错误 | 检查该子命令帮助和筛选有效值后纠正 |
| 3 | 未登录、失效或拒绝授权 | 需要登录时引导官网设备授权；用户拒绝时停止 |
| 4 | 权限或会员能力不足 | 说明实际限制，提供已取结果，不尝试绕过 |
| 5 | 记录不存在或不属于本人 | 说明无法访问，不猜测其他 ID |
| 6 | 网络、限流等故障 | CLI 已有限重试；可在合理间隔后重试一次，仍失败则说明问题 |
| 7 | 部分完成或遍历上限 | 保留已取数据，说明 `complete=false` 和下一页，不称全量 |
| 130 | 用户中断 | 停止，不自动重启 |

stdout 是结果，stderr 包含授权指引、进度或警告。不要把 stderr 的链接/用户码解析成业务数据。CLI 不提供自动投递、数据写入、token 导出或自动续期命令。
