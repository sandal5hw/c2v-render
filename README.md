# c2v-render

c2v 纪录片管线的渲染宿主仓库（2026-11-26 从 sandal5/c2v 迁出）。

- 这里只有 GitHub Actions 工作流，没有制作代码。每个工作流运行时
  把 `sandal5/c2v` 的 `john` 分支 checkout 下来再渲染（c2v 是公开
  仓库，跨仓库拉取不需要额外授权）。
- 迁出的原因：sandal5 账号下还有其他仓库在用 Actions，分钟和
  0.5 GB 存储是整个账号共享的；渲染搬到 sandal5hw 名下后互不影响。
  本仓库是公开仓库，Actions 分钟不限量。
- 全部工作流只接受 `workflow_dispatch` 派发（由 codev 工作台
  https://codev.nbfun.com 发起），没有 push 触发——推代码不会误触
  渲染。样片与正片共用并发组 `c2v-render` 串行排队（一次只渲一个）。
- 工作流清单：
  - `voice.yml` 配音（Fish Audio TTS → codev）
  - `sample-render.yml` 通用样片（960×540）
  - `render-sc008-sample.yml` / `render-bc001-sample.yml` 单集样片（旧）
  - `4k-render.yml` 通用 4K 正片（9 片并行，R2 中转）
  - `4k-render-chunks.yml` 4K 分片（旧）
  - `render-as001-4k.yml` AS001 专用 4K（旧）

需要的仓库 Secrets（值不在本仓库任何文件里）：
`CLOUDFLARE_API_TOKEN`、`CLOUDFLARE_ACCOUNT_ID`、`R2_TOKEN_ID`、
`DIRECTOR_TOKEN`、`C2V_READ_TOKEN`（拉取私有仓库 sandal5/c2v 的
只读令牌）；`FISH_API_KEY` 可选（没有时 voice 工作流会从
codev Worker 取）。
