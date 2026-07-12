# estimate-contract-app プロジェクト

## 作業対象

- 正本はリポジトリ直下の `index.html`。
- HTML/CSS/JavaScriptで構成された単体HTMLアプリ。
- Firebase認証とログイン機能は現時点で未実装。
- OneDrive上のHTMLは利用用コピーであり、正本ではない。

## 作業ルール

- Claude CodeとCodexを対等な開発担当として使用する。
- 修正は正本の `index.html` に対して行う。
- 日付はすべて西暦で表記する。
- 個人名を使わず、「正社員さん」「パートさん」で統一する。
- 変更対象を確認し、対象ファイルだけを個別にステージしてコミット・pushする。
- コミットメッセージは日本語で、`feat: `、`fix: `、`docs: ` 等のプレフィックスを付ける。

## Git作業手順と競合停止

- 開始時に `PROJECT_STATUS.md`、`AGENTS.md`、`CLAUDE.md`、正本情報を確認する。
- `git status` で作業ツリーがクリーン、現在のブランチが `main` であることを確認してから `git pull --ff-only` を実行し、`main` と `origin/main` の同期を確認する。
- 未コミット変更、未取得変更、分岐、競合、他のAIまたは利用者による作業が疑われる場合は、変更を開始せず利用者へ報告する。
- 終了時は差分、テスト結果を確認し、`PROJECT_STATUS.md` を更新してから、対象ファイルだけを個別にステージする。
- 日本語プレフィックス付きでコミットし、push前に再度 `git pull --ff-only` を実行する。失敗、分岐、競合、push拒否があれば停止する。
- push後は `main` と `origin/main` の一致、および `git status` がクリーンであることを確認する。
- `force push`、無断マージ、既存変更の破棄・上書きを行わない。

## 利用者承認が必要な変更

- 仕様、料金、帳票項目、本番公開、デプロイ設定、既存データ、出力形式を変更する場合は、影響範囲と確認・復旧方法を提示し、利用者の明確な承認を得てから実施する。
- 依頼範囲を超える変更は行わず、判断が必要な場合は停止して確認する。

## Git管理対象

- `index.html`
- `README.md`
- `.nojekyll`
- `AGENTS.md`
- `CLAUDE.md`
- `PROJECT_STATUS.md`
- `.gitignore`

## Git管理対象外

- Word契約書テンプレート
- 料金表・見積例Excel
- `outputs/` 以下
- 正本ではない旧版・作業用HTML
- 顧客情報を入力して生成したファイル

非公開資料は将来SharePointの非公開フォルダへ移動予定。現時点ではファイルを移動・削除・リネームしない。プロジェクトの引継ぎ状況と残課題は `PROJECT_STATUS.md` を参照する。

## 公開先

- 公開URL: https://hiro-saitoh-sr.github.io/estimate-contract-app/
- リポジトリ: https://github.com/hiro-saitoh-sr/estimate-contract-app
