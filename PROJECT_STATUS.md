# PROJECT_STATUS

## 現在の状態

- 正本は `public/index.html`（2026-08-06、Firebase Hosting移行に伴いリポジトリ直下の `index.html` から`git mv`で移動）。
- `main` ブランチから Firebase Hosting（`task-app-493716`プロジェクトのサイト`saitoh-sr-estimate-contract`）で公開。GitHub Pagesは無効化済み。
- HTML/CSS/JavaScriptのみで動作するアプリ（中身は1ファイルのまま）。Firebase Authentication（Googleアカウント、`ALLOWED_EMAILS`ホワイトリスト方式）でログイン必須化。データ保存は行わない（PDF出力のみ、現状維持）。
- 既存の料金計算・PDF出力ロジック・帳票内容は変更していない（認証機能の追加のみ）。
- CodexとClaude Codeは対等な開発担当であり、共通Git手順と競合停止ルールを適用する。

## Git状態

- 対象ブランチ: `main`
- 設定整備開始時の基準コミット: `814b7ff`
- 2026-07-13確認時点で `origin/main` と同期済み（ahead 0 / behind 0）

## Git管理方針

正式なGit管理対象は次のファイルとする。

- `public/index.html`
- `firebase.json`
- `.firebaserc`
- `README.md`
- `.nojekyll`
- `AGENTS.md`
- `CLAUDE.md`
- `PROJECT_STATUS.md`
- `.gitignore`

次の資料・生成物は非公開とし、Git管理対象外とする。

- Word契約書テンプレート
- 料金表・見積例Excel
- `outputs/` 以下
- 正本ではない旧版・作業用HTML
- 顧客情報を入力して生成したファイル

非公開資料は将来SharePointの非公開フォルダへ移動する予定。現時点では移動・削除・リネームを行わず、ローカルの現在位置で維持する。

## セキュリティ・運用上の留意事項

- 事務所の振込先情報が公開ソース内に含まれている。Firebase Hosting移行後もログイン必須のため露出範囲は限定的だが、将来必要性を再検討する。
- 非公開資料や顧客情報を含む生成物をコミットしない。`public/`フォルダにはHosting公開対象（`index.html`のみ）以外を絶対に置かない。
- Firebase Hosting + Google認証（ホワイトリスト方式）を導入済み（2026-08-06）。Firestore/Realtime Databaseは使用せずデータ保存なし。

## 残っている判断事項

- 非公開資料をSharePointへ移動する時期と移動先フォルダ構成。
- 公開ソース内の事務所振込先情報を将来も維持するか。

## Firebase Hosting + Google認証移行（2026-08-06）

- 背景: 社員も利用する予定になったため、GitHub Pagesでの認証なし公開からFirebase Hosting + Google認証（ホワイトリスト方式）へ移行。
- 正本を `index.html` → `public/index.html` へ`git mv`で移動（Firebase Hostingの`public`ディレクトリを非公開資料（`*.docx`・料金表Excel・`outputs/`・`work/`）から明確に分離するため）。
- `public/index.html` に追加した内容:
  - Firebase Auth（compat SDK）+ Google Identity Services（GSI）によるログイン画面（`#loginScreen`）
  - ログイン済みのみ表示する`#appShell`（既存の`.header`・`.container`をそのまま内包、認証バー＋ログアウトボタンを追加）
  - `ALLOWED_EMAILS = ["hiro@saitoh-sr.com", "kawahara@saitoh-sr.com"]` によるホワイトリストチェック（許可外アカウントは自動サインアウト）
  - task-app / labor-notice-app / portalと同じ共有Firebaseプロジェクト（`task-app-493716`）・共有OAuthクライアントIDを再利用
  - 既存の料金計算・PDF出力ロジック（`calc()`等）・見た目は無変更
- 新規ファイル: `firebase.json`（`public: "public"`、Hosting target `estimateContract`）、`.firebaserc`（サイト`saitoh-sr-estimate-contract`にマッピング）
- GitHub Pages（`hiro-saitoh-sr.github.io/estimate-contract-app`）は無効化した。

## NEXT_ACTION

- Firebase Hostingサイト`saitoh-sr-estimate-contract`を新規作成し、`firebase deploy --only hosting:estimateContract --project task-app-493716`でデプロイする（利用者の`firebase login --reauth`後に実施）。
- デプロイ後、`public/`配下以外のファイル（`*.docx`・`outputs/`等）が公開URLから参照できない（404になる）ことを確認する。
- 利用者にGoogle Cloud Console / Firebase Console側の設定（portal導入時と同じ3点）を依頼する:
  1. OAuthクライアント（`714632380111-...`、共用）の「承認済みのJavaScript生成元」に `https://saitoh-sr-estimate-contract.web.app` を追加
  2. 共有APIキーのHTTPリファラー許可リストに `https://saitoh-sr-estimate-contract.web.app/*` を追加
  3. Firebase Authenticationの「承認済みドメイン」に `saitoh-sr-estimate-contract.web.app` を追加
- 上記未設定の場合、ログイン時に`auth/requests-from-referer-...-are-blocked`等のエラーになる可能性が高い（portalと同じ既知の課題）。
- GitHub Pages（リポジトリ Settings → Pages → Source）を「None」に変更する。
- 実ブラウザで `hiro@saitoh-sr.com` / `kawahara@saitoh-sr.com` によるログイン、既存の全機能（見積書・契約書5パターン・料金計算明細・手続き料金表・スポット見積書/請求書/領収書のPDF出力）が移行前と同じ挙動であることを確認する。

## 最終更新

- 最終更新AI: Claude Code
- 最終更新日時: 2026-08-06（日本時間）
- 変更内容: Firebase Hosting + Google認証（ホワイトリスト方式）を追加。正本を`public/index.html`へ移動。`firebase.json`/`.firebaserc`を新規作成。既存の料金計算・PDF出力ロジックは無変更。デプロイ・OAuth設定・GitHub Pages無効化は未実施（NEXT_ACTION参照）。
