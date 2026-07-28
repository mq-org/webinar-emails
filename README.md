# webinar-emails

on-line-school.jp のウェビナー「開催日時後」フォローアップメール一覧を表示する閲覧専用ページ。

## 用途
スタッフがウェビナー受講者からの問い合わせ対応時に、「終了後にどんなメールが届いているか」「特典は何か」「アーカイブURLはあるか」を素早く確認するためのリファレンス。

## 構成
- `index.html` - 表示ページ（Tailwind CDN、フェッチで `data/webinars.json` を読み込み）
- `data/webinars.json` - メールデータ（自動更新スクリプトが書き換える）

## 更新方法
ゆみのMacで**毎日9:00に自動実行**（launchd: `com.yumi.webinar-emails-update`）。
WordPress側に変更があった日だけ commit & push されるので、変更がなければ何も起きません。

手動で走らせたいときは:
```
bash ~/Desktop/green_of_life/yumi-ccws/02_scripts/online-school-daily-update.sh
```

処理の中身（`~/Desktop/green_of_life/yumi-ccws/02_scripts/`）:
- `online-school-daily-update.sh` - 取得→生成→commit&push の一連（launchdから呼ばれる）
- `online-school-fetch-targets.mjs` - 対象シナリオの「開催日時後」メールをWPから取得
- `online-school-build-from-full.py` - JSONを整形してこのリポへ配置（商品名・特典LPのoverrideもここ）

**シナリオを増やす/減らすとき:**
- 追加 → `node 02_scripts/online-school-fetch-targets.mjs <シナリオID>` を1回実行（以降は自動更新の対象に入る）
- 除外 → `online-school-build-from-full.py` の `EXCLUDE_SCENARIO_IDS` に追記

**WPのログインが切れたら:** 自動更新は空データを書かずに中止し、ログに警告が出ます。
`node 02_scripts/online-school-save-session.mjs` でログインし直してください。
ログ: `02_scripts/.cache/online-school/update.log`

## デプロイ
Vercel自動連携。`main`ブランチへのpushで自動再ビルド。

## 編集について
**Vercel側で内容を手で書き換えても、WordPress本体は変わらず、次の自動更新で上書きされます。**
メール文面・特典内容を変更したい場合は、必ず元となる WordPress（on-line-school.jp）または LP側で編集してください。
