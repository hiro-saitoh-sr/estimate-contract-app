# estimate-contract-app プロジェクト

## プロジェクト概要

社会保険労務士事務所の見積書・契約書作成アプリ。月次顧問契約・スポット手続きの見積書、業務委託契約書、料金計算明細、月次報酬一覧＋手続き料金表、スポット手続きの見積書・請求書を作成する。

## 公開URL・リポジトリ

- 公開URL: https://hiro-saitoh-sr.github.io/estimate-contract-app/
- リポジトリ: https://github.com/hiro-saitoh-sr/estimate-contract-app

## 開発・管理方針

- 正本HTMLはリポジトリ直下の `index.html`。
- HTML/CSS/JavaScriptの単体HTMLアプリで、Firebaseは使用せず、ログインは不要。
- 修正後は `git add . && git commit && git push` を実行する。
- コミットメッセージは日本語で、`feat: `、`fix: `、`docs: ` 等のプレフィックスを付ける。
- OneDrive上のHTMLは利用用コピーであり、正本ではない。
- 個人名は使用せず、「正社員さん」「パートさん」で統一する。
- 日付は和暦を使わず、すべて西暦とする。

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

非公開資料は将来SharePointの非公開フォルダへ移動予定。移動が決まるまでは、現在の内容・場所・ファイル名を維持する。詳細な引継ぎ状況は `PROJECT_STATUS.md` を参照する。

## 主な機能

- 顧問契約用の月次報酬見積書作成
- 業務委託契約書作成（A / A+B / C / A+C / A+B+C）
- 料金計算明細出力
- 月次報酬一覧＋手続き料金表出力
- スポット手続きの見積書・請求書作成

## 関連プロジェクト

- task-app: 社労士業務タスク管理システム
- shogu-app: 処遇改善加算タスク管理システム
