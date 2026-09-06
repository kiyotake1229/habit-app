# コツコツ iOSアプリ化ガイド（Capacitor）

今の完成済みWebアプリ（`../index.html`）をそのままネイティブiOSアプリにして、App Storeへ申請するための手順です。
土台（Capacitorプロジェクト・ネイティブ通知/触覚の組み込み）はすでに構築済みです。

---

## 全体像

- **方式**: Capacitor で今の `index.html` をネイティブアプリに同梱
- **ネイティブ強化済み**: ローカル通知（閉じていても定時に届く）／触覚フィードバック（本物の振動）
- **App ID**: `work.ltv.kotsu`（変更可 → `capacitor.config.json`）
- **アプリ名**: コツコツ

---

## STEP 1〜3：あなたの作業（必須・私は代行不可）

### STEP 1. macOSを26.2以降にアップグレード
- このMac（M1 MacBook Air）は対応しています。
- **必ず事前にTime Machineでバックアップ**してください。
- 「システム設定 → 一般 → ソフトウェアアップデート」から実行。1〜2時間程度。

### STEP 2. Xcodeをインストール
- App Storeで「Xcode」を検索してインストール（約15GB）。
- 完了後、ターミナルで一度だけ：
  ```bash
  sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
  sudo xcodebuild -license accept
  ```
- CocoaPodsを入れる（未導入なら）：
  ```bash
  brew install cocoapods
  ```

### STEP 3. Apple Developer Programに登録（年 $99）
- https://developer.apple.com/programs/ から本人のApple IDで登録。
- 支払い・本人確認が必要。承認まで最短で当日〜数日。

---

## STEP 4：ビルド（Xcode導入後・私も手伝えます）

このフォルダ（`ios-app/`）で：

```bash
npm install          # 依存の取得（初回のみ／実行済み）
npm run add-ios      # www同期 → iOSプロジェクト生成 → 同期
npm run icons        # アプリアイコンを自動生成（../icon-512.png から）
npm run open         # Xcodeで開く
```

Webアプリ側（`../index.html`）を修正したら、反映は：
```bash
npm run sync
```

## STEP 5：Xcodeで署名して実機/シミュレータ確認

1. Xcodeが開いたら、左の「App」→「Signing & Capabilities」
2. **Team** に STEP3 で登録したアカウントを選択（自動署名ON）
3. 「Signing & Capabilities」→ **＋Capability** で **Push Notifications** は不要。ローカル通知のみなので追加設定は不要です。
4. 上部の実行先を実機かシミュレータにして ▶ で起動 → 動作確認

## STEP 6：App Store申請

1. https://appstoreconnect.apple.com でアプリを新規作成（Bundle ID = `work.ltv.kotsu`）
2. Xcodeで「Product → Archive」→ Organizerから **Distribute App → App Store Connect** でアップロード
3. App Store Connectで以下を入力（私が下書きを用意できます）：
   - アプリ名 / サブタイトル / 説明文 / キーワード
   - スクリーンショット（6.7インチ・6.5インチ・5.5インチ等）
   - プライバシー: **データ収集なし**（すべて端末内localStorageのみ）で申請可
   - サポートURL（例: GitHub PagesのURL）
4. 審査へ提出（通常1〜3日）

---

## 審査で気をつける点（重要）

- Appleガイドライン **4.2（最低限の機能）**: 単なるWebサイトの殻は却下されます。
  本アプリは**オフライン動作・端末内データ・ネイティブ通知/触覚**を備えた自立アプリなので問題になりにくいですが、
  申請時の説明で「習慣管理を完結して行える独立アプリ」であることを明確にしてください。

## メモ
- `npm audit` の警告は開発用ツール（アイコン生成の `@capacitor/assets`）の依存にあるもので、
  **アプリ本体には含まれません**。無視して問題ありません。
- `www/` と `ios/` の生成物、`node_modules/` は `.gitignore` 済み。
