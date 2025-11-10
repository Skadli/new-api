# 品牌资源放置说明（中文）

将你的品牌资源放到本目录，流水线会在同步上游后自动覆盖：

- `branding/favicon.ico` → 覆盖 `web/favicon.ico`
- `branding/logo.png` → 覆盖 `web/logo.png`

注意事项：
- 请确保文件名与后缀与上述一致。
- 若任一文件缺失，将跳过对应覆盖步骤，不会报错。
- 文案替换默认仅在 `web/` 目录下执行，将所有 `NEW-api` 替换为 `meAPI`；
  如需修改替换词或范围，可在仓库变量中设置：
  - `REPLACE_FROM`（默认：`NEW-api`）
  - `REPLACE_TO`（默认：`meAPI`）

