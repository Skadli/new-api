# 同步上游与品牌定制说明

本仓库内置工作流会每周同步上游仓库 `https://github.com/QuantumNous/new-api`，
并自动应用以下改动：

- 覆盖 `web/public/favicon.ico` 与 `web/public/logo.png`（来源：`branding/` 目录）
- 在 `web/` 目录内将所有 `new-api` 文本替换为 `meAPI`
- 将`web/`目录下的所有`/logo.png`替换为`https://r2.690990.xyz/logo.png`
- 将`web/index.html`中的`content="OpenAI 接口聚合管理，支持多种渠道包括 Azure，可用于二次分发管理 key，仅单可执行文件，已打包好 Docker 镜像，一键部署，开箱即用"`替换为`content="高速稳定的AI API代理平台，支持国内外所有主流AI模型API接入，无论GPT、Claude、gemini或其他模型均可快速调用，轻松实现全模型接入，保障高可用性与低延迟。"`  和 `<title>New API</title>`替换为`<title>me API</title>`
- 注释`web/src/components/layout/Footer.jsx`中`<div className='text-sm'>
            <span className='!text-semi-color-text-1'>
              {t('设计与开发由')}{' '}
            </span>
            <a
              href='https://github.com/QuantumNous/new-api'
              target='_blank'
              rel='noopener noreferrer'
              className='!text-semi-color-primary font-medium'
            >
              New API
            </a>
          </div> `和`<div className='absolute bottom-2 right-4 text-xs !text-semi-color-text-2 opacity-70'>
            <span>{t('设计与开发由')} </span>
            <a
              href='https://github.com/QuantumNous/new-api'
              target='_blank'
              rel='noopener noreferrer'
              className='!text-semi-color-primary font-medium'
            >
              New API
            </a>
          </div> `两段代码
- 注释`web/src/index.jsx`中的`if (typeof window !== 'undefined') {
  console.log(
    '%cWE ❤ NEWAPI%c Github: https://github.com/QuantumNous/new-api',
    'color: #10b981; font-weight: bold; font-size: 24px;',
    'color: inherit; font-size: 14px;',
  );
}`代码

## 使用步骤

1. 将你的品牌资源放入：
   - `branding/favicon.ico`
   - `branding/logo.png`

2. 手动触发一次工作流（Actions → 每周同步上游并应用品牌 → Run workflow），验证结果。

## 运行机制

- 工作流文件：`.github/workflows/sync-upstream.yml`
- 同步逻辑：已内联至 `.github/workflows/sync-upstream.yml`，无需额外脚本
- 采用浅克隆拉取上游，使用 `rsync` 覆盖到仓库根目录，排除以下路径：
  - `.github/`（保留本仓库工作流）
  - `branding/`（保留品牌资源）
  - `tools/`（保留同步脚本）
- 若无变更，将跳过提交与推送。
 - 推送策略：默认快进推送；如远端分支领先，将自动 `git fetch && git rebase` 并重试最多 3 次；若仍冲突，则尝试 `--force-with-lease`（受保护分支会被拒绝），建议改走 PR。

## 自定义

- 若需扩大替换范围，可在工作流中调整 `web_dir` 变量或 `grep -rIl` 查找范围。
- 若需支持大小写/多变体替换，可使用更复杂的 `sed` 正则或多次替换。

## 注意事项

- 若 `main` 为受保护分支，请使用非受保护分支运行同步，并通过 PR 合并。
- 上游仓库结构变更（例如 `web/` 重命名）时，请同步修改脚本中的路径变量。
