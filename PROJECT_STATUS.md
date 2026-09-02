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
- 月次報酬一覧（`monthlyFeeTableHtml()`）の「月次オプション」h2直前に`<div class="page-break"></div>`を追加（2026-08-28）。改ページ位置の修正のみで、金額・項目・計算ロジックの変更はなし。Firebase Hostingへデプロイ済み（2026-08-28）。
- 料金計算明細（`detailBreakdownHtml()`）の「月次オプション」テーブル直前にも`<div class="page-break"></div>`を追加（2026-08-28）。既存の`.page-break`クラスを利用。改ページ位置の修正のみで、金額・項目・計算ロジックの変更はなし。Firebase Hostingへデプロイ済み（2026-08-28）。
- PDF出力ボタンを個別出力（見積書・契約書・料金計算明細・手続き料金表）から3ボタンに再編（2026-08-29）。①「📋 確認用セット出力」＝見積書→料金明細→契約書→料金表の順に1ウィンドウへ連結して出力、②「✍️ 締結用セット出力」＝契約書→料金明細→料金表の順に連結して出力、③「💰 料金表を出力」＝手続き料金表単独出力（`printFeeTable()`、内容・スタイルとも現状維持）。各帳票のHTML生成部を`estimateContentHtml()`／`detailContentHtml()`／`contractContentHtml()`／`feeTableBodyHtml()`として関数分離し、セット出力側で`<div class="page-break">`を挟んで連結する実装。スポット手続き（見積書・請求書・領収書）のボタン・ロジックは変更なし。料金計算ロジックは変更なし。
- 契約開始日の入力を`input type="text"`から`input type="date"`（カレンダー選択）に変更（2026-08-29）。契約開始日を選択すると`onContractStartChange()`が契約終了日に「翌年7月31日」（開始日の年+1年の7/31）を自動入力する。契約終了日は引き続き`type="text"`のまま手動編集可能（自動入力後に上書き可）。既存の日付表示ロジック（`contractDateFormatter()`、旧`toW`）はスラッシュ・ハイフン両区切りに対応済みのため変更なし。
- セット出力・料金表単体出力のファイル名（印刷ウィンドウの`<title>`）を変更（2026-08-29）。確認用セット＝`社会保険労務士齊藤事務所_お見積書一式_{顧問先名}様_{日付}`、締結用セット＝`社会保険労務士齊藤事務所_業務委託契約書_{顧問先名}様_{日付}`。料金表単体（`printFeeTable()`）は`社会保険労務士齊藤事務所_手続き料金表_{日付}`のまま現状維持。
- 確認用セット出力のみ、契約書タイトルを「業 務 委 託 契 約 書」→「業 務 委 託 契 約 書（案）」に変更（2026-08-29）。`printConfirmationSet()`内で契約書HTMLの当該文字列を置換する実装（`getContract1`〜`5`本体・締結用セット・単体の`printContract()`は無変更）。
- 業務委託契約書テンプレート`getContract5`（労務＋給与計算＋処遇改善加算サポート、A+B+Cパターン）の「3．処遇改善加算サポート契約」タイトル直前に`<div class="page-break"></div>`を追加（2026-08-29）。この見出し文字列は5パターン中`getContract5`にのみ存在するため他パターンへの変更はなし。5パターン全ての動作確認済み。
- 料金表（月次報酬一覧＋手続き料金表）の印刷専用スタイルを`.fee-table-section`クラスにスコープする形に変更し、単体出力・セット出力（確認用・締結用）の料金表部分で同一のフォントサイズ・行間・表の余白スタイルを適用するよう統一（2026-08-29）。単体出力（`printFeeTable()`）は従来どおり`@page`余白12mmも適用（現状維持）。セット出力側はページ内の他帳票（見積書・料金明細・契約書）の余白に影響しないよう、`@page`（ページ余白）の変更は含めていない。
- 手続き料金表の注釈末尾に「※一覧にない手続きについても対応できる場合がございます。お気軽にご相談ください。」「※料金は案件の難易度・複雑さにより別途お見積りとなる場合があります。」の2行を追加（2026-08-29）。金額・項目・計算ロジックの変更はなし。
- 業務委託契約書テンプレート（`getContract1`〜`5`）の【給与計算サポート契約の確認事項】を修正（2026-08-30）。「勤怠システムを使用されない場合は乙の指定するExcelに甲にてご入力いただきます」「甲は給与計算に必要な情報を給与支給日の12日前までに乙へ提供します」の2行を削除し、「勤怠の集計はお客様にて行っていただきます。給与計算に必要な勤怠情報・賃金変動データ等は、乙の指定するExcelにご入力のうえ、支給日の12日前までにご提供ください。」の1行に統合。給与計算サポート契約（B）を含むパターンのみ該当（共通関数`payrollClauseHtml()`＝`getContract2`、`getContract5`内の同文言）。B契約を含まない`getContract1`・`3`・`4`にはこの文言自体が存在しないため変更なし。他の条項・料金計算ロジックは変更なし。
- CodexとClaude Codeは対等な開発担当であり、共通Git手順と競合停止ルールを適用する。
- 労務サポート契約（A）の基本料を固定15,000円から可変入力に変更（2026-09-01）。チェックボックスラベルを「労務サポート契約（基本料＋従業員数単価）」に変更し、チェック時のみ表示される「労務サポート基本料（税抜）」入力欄（`serviceABaseAmount`、デフォルト15,000円・`min=0`・`step=1000`）を追加。`calc()`が`getServiceABase()`で入力値を読み取り`calcResult.aBase`に格納、料金計算結果・見積書（`estimateContentHtml()`）・料金計算明細（`detailBreakdownHtml()`）・業務委託契約書5パターン（`getContract1`・`2`・`4`・`5`、Aを含まない`getContract3`は対象外）の基本料表示に反映。月次報酬一覧（`monthlyFeeTableHtml()`）は固定15,000円のまま変更なし。処遇改善加算サポート（C）の基本料（Aあり5,000円／Aなし15,000円）は独立した固定値のため計算ロジック変更なし。デフォルト値15,000円のため既存動作は変わらない。Node（vm）でDOMをスタブした単体テストにより、デフォルト値での既存動作再現・カスタム値の計算反映・C基本料の非干渉を確認済み（ブラウザでのログイン後の実機確認は未実施）。Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ済み（2026-09-01、`firebase deploy --only hosting:estimateContract --project task-app-493716`、「found 1 files in public」でindex.html以外は含まれないことを確認）。
- 料金表出力の労務サポート基本料表記を用途別に分離（2026-09-01）。`monthlyFeeTableHtml()`・`feeTableBodyHtml()`・`feeTableSectionHtml()`に`aBaseLabel`引数を追加し、呼び出し元で表示文言を指定する方式に変更。単体の「料金表を出力」ボタン（`printFeeTable()`）は固定表記「15,000円〜」を渡すのみで入力値とは連動しない。確認用セット（`printConfirmationSet()`）・締結用セット（`printSigningSet()`）は`(r.hasA ? r.aBase : 15000).toLocaleString() + '円'`で、労務サポート契約（A）にチェックがあれば入力値、チェックがなければデフォルト値「15,000円」を表示する。B（給与計算）・C（処遇改善加算）の料金表部分・既存の料金計算ロジックは変更なし。Node（vm）でDOMをスタブした単体テストにより、単体出力が常に「15,000円〜」・セット出力がA未チェック時「15,000円」／チェック時は入力値（例：22,000円）になること、B・C部分が変化しないことを確認済み（ブラウザでのログイン後の実機確認は未実施）。Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ済み（2026-09-01、`firebase deploy --only hosting:estimateContract --project task-app-493716`、「found 1 files in public」でindex.html以外は含まれないことを確認）。
- 月次報酬一覧（単体の「料金表を出力」ボタンのみ）に労務サポート基本料に関する注釈を追加（2026-09-01）。`monthlyFeeTableHtml()`・`feeTableBodyHtml()`・`feeTableSectionHtml()`に`includeABaseNote`引数を追加し、`true`が渡されたときのみ既存の注釈末尾（「源泉所得税は報酬額（税抜）を基準に計算します。」の後）に「※労務サポート基本料は業務内容・契約内容により異なります。詳細はお見積書をご確認ください。」を追加する方式に変更。`printFeeTable()`のみ`feeTableSectionHtml('15,000円〜', true)`で`true`を渡し、確認用セット（`printConfirmationSet()`）・締結用セット（`printSigningSet()`）は第2引数を渡さない（`undefined`のため注釈は表示されない）。月次報酬一覧はコード上3つの出力（単体・確認用セット・締結用セット）で共通の関数を使うため、表示範囲について利用者に確認したところ「単体出力のみ」との回答を得て実装。B（給与計算）・C（処遇改善加算）の料金表部分・既存の料金計算ロジックは変更なし。Node（vm）による単体テストで、単体出力（`includeABaseNote=true`）にのみ注釈が末尾に追加され、セット出力相当（引数省略・`false`明示の両方）では追加されないことを確認済み（ブラウザでのログイン後の実機確認は未実施）。Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ済み（2026-09-01、`firebase deploy --only hosting:estimateContract --project task-app-493716`、「found 1 files in public」でindex.html以外は含まれないことを確認）。
- 確認用セット（`printConfirmationSet()`）・締結用セット（`printSigningSet()`）の料金表で、労務サポート契約（A）未チェック時の基本料表記を「15,000円」から「15,000円〜」に変更（2026-09-02）。`setABaseLabel`の算出を`(r.hasA ? r.aBase : 15000).toLocaleString() + '円'`から`r.hasA ? (r.aBase.toLocaleString() + '円') : '15,000円〜'`に変更。チェックあり時（入力値表示）の挙動は変更なし。単体の「料金表を出力」ボタン（`printFeeTable()`、固定表記「15,000円〜」＋注釈）・B（給与計算）／C（処遇改善加算）の料金表部分・既存の料金計算ロジックは変更なし。Node上での埋め込みスクリプト構文チェック済み（ブラウザでのログイン後の実機確認は未実施）。Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ済み（2026-09-02、`firebase deploy --only hosting:estimateContract`、「found 1 files in public」でindex.html以外は含まれないことを確認）。

## Git状態

- 対象ブランチ: `main`
- 直近コミット: `096590c`（一式セットの料金表で労務サポート未選択時の基本料を「15,000円〜」に変更）
- 2026-09-02確認時点で `origin/main` と同期済み（ahead 0 / behind 0）、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了

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
- 最終更新日時: 2026-08-30（日本時間）
- 変更内容: 業務委託契約書テンプレート（`getContract1`〜`5`）の【給与計算サポート契約の確認事項】を修正。「勤怠システムを使用されない場合は乙の指定するExcelに甲にてご入力いただきます」「甲は給与計算に必要な情報を給与支給日の12日前までに乙へ提供します」の2行を削除し、「勤怠の集計はお客様にて行っていただきます。給与計算に必要な勤怠情報・賃金変動データ等は、乙の指定するExcelにご入力のうえ、支給日の12日前までにご提供ください。」の1行に統合。給与計算サポート契約（B）を含む`getContract2`（共通関数`payrollClauseHtml()`）・`getContract5`（同文言をインラインで保持）の2箇所に適用。B契約を含まない`getContract1`・`3`・`4`にはこの文言自体が存在しないため変更なし。他の条項・料金計算ロジックは変更なし。jsdomによるヘッドレステストで契約書5パターン全て（B契約を含む2パターンで新文言への置換、含まない3パターンで無変更）を確認済み（実ブラウザでのGoogleログイン経由の目視確認は未実施）。コミット`09e1983`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

### 過去の更新

- 2026-08-29（Claude Code）: ①セット出力・料金表単体出力のファイル名を変更（確認用セット＝`お見積書一式`、締結用セット＝`業務委託契約書`、料金表単体は現状維持）。②確認用セットのみ契約書タイトルを「業 務 委 託 契 約 書（案）」に変更（締結用セット・単体`printContract()`は無変更）。③`getContract5`（A+B+Cパターン）の「3．処遇改善加算サポート契約」タイトル直前に改ページを追加（この見出しは`getContract5`にのみ存在するため他4パターンはコード変更なし、5パターン全て動作確認済み）。④料金表の印刷スタイルを`.fee-table-section`クラスにスコープし、単体出力・セット出力の料金表部分で同一スタイル（フォントサイズ・行間・表の余白）に統一（`@page`余白は単体出力のみ12mm、セット出力の他帳票には影響させない設計）。⑤手続き料金表の注釈末尾に2行追加（対応可否・追加見積りの案内）。既存の料金計算ロジック・スポット手続き出力（見積書/請求書/領収書）は変更なし。コミット`22c2fa9`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

- 2026-08-29（Claude Code）: PDF出力ボタンを「①📋確認用セット出力（見積書→料金明細→契約書→料金表）」「②✍️締結用セット出力（契約書→料金明細→料金表）」「③💰料金表を出力（現状維持）」の3ボタンに再編。契約開始日を`type="date"`に変更し、選択時に契約終了日へ翌年7月31日を自動入力（手動修正は可能）。既存の料金計算ロジック・帳票内容・スポット手続き出力は変更なし。コミット`9618e19`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

- 2026-08-28（Claude Code）: 料金計算明細（`detailBreakdownHtml()`）の「月次オプション」テーブル直前に`<div class="page-break"></div>`を追加。既存の`.page-break`クラス（`break-before: page`）を利用した改ページ位置の修正のみで、金額・項目・計算ロジックの変更はなし。コミット`7b10782`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

- 2026-08-28（Claude Code）: 月次報酬一覧（`monthlyFeeTableHtml()`）の「月次オプション」h2直前に`<div class="page-break"></div>`を追加。改ページ位置の修正のみで、金額・項目・計算ロジックの変更はなし。コミット`d805188`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

- 2026-08-21（Claude Code）: 料金計算明細（`printDetail()`）の顧問先名表示を「顧問先名：株式会社〇〇」から「株式会社〇〇 御中」に変更。ラベル「顧問先名：」を削除し、社名の後に「 御中」を付与。金額・計算ロジック・他の帳票は変更なし。コミット`7cd9aaa`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了。

- 2026-08-14（Claude Code）: 業務委託契約書テンプレート（`getContract1`〜`5`）の署名欄・改ページを修正。①甲側の署名欄を「代表者」ラベル＋空白の署名スペースがある元の形式に戻した（前回改訂で削除していたもの。乙側「代表者　齊藤 広幸」は維持）、②`.section`（各条タイトル）に`break-after: avoid`を追加し、タイトルのみ前ページに残らないようにした、③全5パターンの「報酬額」条の直前に`<div class="page-break">`を挿入、④全5パターンの署名欄（`signatureBlock`）の直前に`<div class="page-break">`を挿入。5パターン全てに適用。料金計算・PDF出力ロジックは変更なし。ブラウザでの目視確認後、コミット`f070a2e`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了（`found 1 files in public`でindex.html以外は含まれないことを確認）。

- 2026-08-14（Claude Code）: 業務委託契約書テンプレート（`getContract1`〜`5`）を改訂。①条番号を1から連番で整理（新設の「報酬の改定」条を含め、重複・抜けを解消）、②「補足」を条番号なし・背景色なしの`.section-note`表記（［補足］）に変更、③【労務サポート契約の確認事項】に台帳登録変更時の文言を追記、④【処遇改善加算サポート契約の確認事項】に健康保険料・介護保険料合算表示時の取り扱いを追記、⑤共通確認事項の後・契約期間の前に「報酬の改定」条（改定日の２ヶ月前までに書面又は電磁的方法により通知）を新設、⑥署名欄の甲側「代表者」空白行（押印欄）を削除し電子署名のみの形式に統一（乙側の「代表者　齊藤 広幸」は維持）。5パターン全てに適用。料金計算・PDF出力ロジックは変更なし。ブラウザでの目視確認後、コミット`cf439c3`をpush、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了（`found 1 files in public`でindex.html以外は含まれないことを確認）。

- 2026-08-14（Claude Code）: `printFeeTable()`に印刷専用スタイル（`feeTableStyle`）を追加し、月次報酬一覧・手続き料金表の印刷/PDF出力が2ページ以内に収まるよう調整。金額・帳票項目・計算ロジックの変更はなし。コミット`d6bf14a`をpush後、Firebase Hosting（`saitoh-sr-estimate-contract`）へデプロイ完了（`found 1 files in public`でindex.html以外は含まれないことを確認）。
- 2026-08-06（Claude Code）: Firebase Hosting + Google認証（ホワイトリスト方式）を追加し、Firebase Hostingサイト`saitoh-sr-estimate-contract`を作成・デプロイ完了。正本を`public/index.html`へ移動。`firebase.json`/`.firebaserc`を新規作成。既存の料金計算・PDF出力ロジックは無変更。OAuth設定・GitHub Pages無効化・実ブラウザでのログイン確認は未実施（NEXT_ACTION参照）。
