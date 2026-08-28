# PROJECT_STATUS

## 現在の状態

- 正本は `public/index.html`（2026-08-06、Firebase Hosting移行に伴いリポジトリ直下の `index.html` から`git mv`で移動）。
- `main` ブランチから Firebase Hosting（`task-app-493716`プロジェクトのサイト`saitoh-sr-estimate-contract`、公開URL https://saitoh-sr-estimate-contract.web.app ）へデプロイ済み（2026-08-06）。GitHub Pagesの無効化はまだ（NEXT_ACTION参照）。
- Google Cloud Console / Firebase Console側のOAuth設定が未実施のため、現時点では公開URLでのGoogleログインは失敗する見込み（NEXT_ACTION参照）。
- HTML/CSS/JavaScriptのみで動作するアプリ（中身は1ファイルのまま）。Firebase Authentication（Googleアカウント、`ALLOWED_EMAILS`ホワイトリスト方式）でログイン必須化。データ保存は行わない（PDF出力のみ、現状維持）。
- 既存の料金計算・PDF出力ロジック・帳票内容は変更していない（認証機能の追加のみ）。
- `printFeeTable()`（月次報酬一覧・手続き料金表の印刷/PDF出力）に印刷専用スタイル（`feeTableStyle`）を追加し、2ページ以内に収まるよう調整（2026-08-14）。金額・帳票項目・計算ロジックの変更はなし。
- 業務委託契約書テンプレート（`getContract1`〜`5`）を改訂（2026-08-14）。条番号の重複・抜けを整理し「報酬の改定」条を新設、【労務サポート契約の確認事項】【処遇改善加算サポート契約の確認事項】に文言を追記。料金計算ロジックは変更なし。
- 業務委託契約書テンプレート（`getContract1`〜`5`）の署名欄・改ページを修正（2026-08-14）。甲側の署名欄は「代表者」ラベル＋空白の署名スペースがある元の形式に戻し（乙側「代表者　齊藤 広幸」は維持）、各条タイトルが前ページに取り残されないよう`.section`に`break-after: avoid`を追加、「報酬額」条の直前・署名欄の直前に必ず改ページを挿入。料金計算ロジックは変更なし。
- 料金計算明細（`printDetail()`）の顧問先名表示を「顧問先名：株式会社〇〇」から「株式会社〇〇 御中」に変更（2026-08-21）。ラベルを削除し社名の後に「 御中」を付与。金額・計算ロジックの変更はなし。
- 月次報酬一覧（`monthlyFeeTableHtml()`）の「月次オプション」h2直前に`<div class="page-break"></div>`を追加（2026-08-28）。改ページ位置の修正のみで、金額・項目・計算ロジックの変更はなし。
- CodexとClaude Codeは対等な開発担当であり、共通Git手順と競合停止ルールを適用する。

## Git状態

- 対象ブランチ: `main`
- 直近コミット: `7cd9aaa`（料金計算明細の顧問先名表示を「御中」形式に変更）
- 2026-08-21確認時点で `origin/main` と同期済み（ahead 0 / behind 0）

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
- Firebase Hostingサイト`saitoh-sr-estimate-contract`を作成し、デプロイ完了（2026-08-06、`firebase deploy --only hosting:estimateContract --project task-app-493716`。「found 1 files in public」でindex.html以外は含まれないことを確認）。
- デプロイ後の確認（2026-08-06実施）: 公開URL直下は200、`outputs/`・`PROJECT_STATUS.md`・`見積契約書作成.html`・`firebase.json`はいずれも404で参照不可。COOPヘッダー（`same-origin-allow-popups`）付与も確認。ページHTML内に`#loginScreen`・`#appShell`・`ALLOWED_EMAILS`の記述があることも確認。
- GitHub Pages（`hiro-saitoh-sr.github.io/estimate-contract-app`）の無効化はまだ実施していない（`gh` CLI未導入のため、利用者がGitHub Web UIで実施する必要がある）。

## NEXT_ACTION

- 利用者にGoogle Cloud Console / Firebase Console側の設定（portal導入時と同じ3点）を依頼する:
  1. OAuthクライアント（`714632380111-...`、共用）の「承認済みのJavaScript生成元」に `https://saitoh-sr-estimate-contract.web.app` を追加
  2. 共有APIキーのHTTPリファラー許可リストに `https://saitoh-sr-estimate-contract.web.app/*` を追加
  3. Firebase Authenticationの「承認済みドメイン」に `saitoh-sr-estimate-contract.web.app` を追加
- 上記未設定の場合、ログイン時に`auth/requests-from-referer-...-are-blocked`等のエラーになる可能性が高い（portalと同じ既知の課題）。上記設定後に実ブラウザでログインを確認する。
- GitHub Pages（リポジトリ Settings → Pages → Source）を「None」に変更する。
- 実ブラウザで `hiro@saitoh-sr.com` / `kawahara@saitoh-sr.com` によるログイン、既存の全機能（見積書・契約書5パターン・料金計算明細・手続き料金表・スポット見積書/請求書/領収書のPDF出力）が移行前と同じ挙動であることを確認する。
- ホワイトリスト外アカウントでのログインが拒否されることを確認する。

## 最終更新

- 最終更新AI: Claude Code
- 最終更新日時: 2026-08-21（日本時間）
- 変更内容: 料金計算明細（`printDetail()`）の顧問先名表示を「顧問先名：株式会社〇〇」から「株式会社〇〇 御中」に変更。ラベル「顧問先名：」を削除し、社名の後に「 御中」を付与。金額・計算ロジック・他の帳票は変更なし。コミット`7cd9aaa`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

### 過去の更新

- 2026-08-14（Claude Code）: 業務委託契約書テンプレート（`getContract1`〜`5`）の署名欄・改ページを修正。①甲側の署名欄を「代表者」ラベル＋空白の署名スペースがある元の形式に戻した（前回改訂で削除していたもの。乙側「代表者　齊藤 広幸」は維持）、②`.section`（各条タイトル）に`break-after: avoid`を追加し、タイトルのみ前ページに残らないようにした、③全5パターンの「報酬額」条の直前に`<div class="page-break">`を挿入、④全5パターンの署名欄（`signatureBlock`）の直前に`<div class="page-break">`を挿入。5パターン全てに適用。料金計算・PDF出力ロジックは変更なし。ブラウザでの目視確認後、コミット`f070a2e`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了（`found 1 files in public`でindex.html以外は含まれないことを確認）。

- 2026-08-14（Claude Code）: 業務委託契約書テンプレート（`getContract1`〜`5`）を改訂。①条番号を1から連番で整理（新設の「報酬の改定」条を含め、重複・抜けを解消）、②「補足」を条番号なし・背景色なしの`.section-note`表記（［補足］）に変更、③【労務サポート契約の確認事項】に台帳登録変更時の文言を追記、④【処遇改善加算サポート契約の確認事項】に健康保険料・介護保険料合算表示時の取り扱いを追記、⑤共通確認事項の後・契約期間の前に「報酬の改定」条（改定日の２ヶ月前までに書面又は電磁的方法により通知）を新設、⑥署名欄の甲側「代表者」空白行（押印欄）を削除し電子署名のみの形式に統一（乙側の「代表者　齊藤 広幸」は維持）。5パターン全てに適用。料金計算・PDF出力ロジックは変更なし。ブラウザでの目視確認後、コミット`cf439c3`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了（`found 1 files in public`でindex.html以外は含まれないことを確認）。

- 2026-08-14（Claude Code）: `printFeeTable()`に印刷専用スタイル（`feeTableStyle`）を追加し、月次報酬一覧・手続き料金表の印刷/PDF出力が2ページ以内に収まるよう調整。金額・帳票項目・計算ロジックの変更はなし。コミット`d6bf14a`をpush後、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了（`found 1 files in public`でindex.html以外は含まれないことを確認）。
- 2026-08-06（Claude Code）: Firebase Hosting + Google認証（ホワイトリスト方式）を追加し、Firebase Hostingサイト`saitoh-sr-estimate-contract`を作成・デプロイ完了。正本を`public/index.html`へ移動。`firebase.json`/`.firebaserc`を新規作成。既存の料金計算・PDF出力ロジックは無変更。OAuth設定・GitHub Pages無効化・実ブラウザでのログイン確認は未実施（NEXT_ACTION参照）。
