# webinar-emails

on-line-school.jp のウェビナー「開催日時後」フォローアップメール一覧を表示する閲覧専用ページ。

## 用途
スタッフがウェビナー受講者からの問い合わせ対応時に、「終了後にどんなメールが届いているか」「特典は何か」「アーカイブURLはあるか」を素早く確認するためのリファレンス。

## 構成
- `index.html` - 表示ページ（Tailwind CDN、フェッチで `data/webinars.json` を読み込み）
- `data/webinars.json` - メールデータ（自動更新スクリプトが書き換える）

## 更新方法
ゆみのMacで週次自動実行（launchd）。緊急時は手動でスクリプト実行も可能。

データソースは `~/Desktop/green_of_life/yumi-ccws/02_scripts/` の以下スクリプト群:
- `online-school-list-target-scenarios.mjs` - 対象作者の最新シナリオ抽出
- `online-school-fetch-after-batch.mjs` - 各シナリオの「開催日時後」メール取得
- `online-school-build-json.py` - JSONを整形してこのリポへ配置

## デプロイ
Vercel自動連携。`main`ブランチへのpushで自動再ビルド。

## 編集について
**Vercel側で内容を手で書き換えても、WordPress本体は変わらず、次の自動更新で上書きされます。**
メール文面・特典内容を変更したい場合は、必ず元となる WordPress（on-line-school.jp）または LP側で編集してください。
