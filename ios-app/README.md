# コツコツ iOSアプリ（Capacitor）

Webアプリ本体（`../index.html`）をネイティブiOSアプリとして同梱したプロジェクトです。

- **申請・アップロードは担当者（上司）が実施** → 手順は [`上司への引き渡し手順.md`](上司への引き渡し手順.md)
- このREADMEは開発側（アプリを作る人）向けのメモです

---

## 現在の状態

| 項目 | 状態 |
|------|------|
| Capacitorプロジェクト | ✅ 構築済み（`work.ltv.kotsu` / コツコツ） |
| Xcodeプロジェクト生成 | ✅ 済み（`ios/App/App.xcworkspace`） |
| Webアプリ同梱 | ✅ 済み（`ios/App/App/public/`） |
| アプリアイコン・スプラッシュ | ✅ 生成済み（1024px・透過なし＝App Store要件OK） |
| ネイティブ通知・触覚 | ✅ 組み込み済み |
| ネイティブ保存(Preferences) | ✅ 組み込み済み（データ消失対策） |
| `pod install` | ✅ 完了済み（6 pods統合済み） |

> Xcodeのある環境で `npm install` 後に `App.xcworkspace` を開けばそのままビルドできます。

---

## 構成

```
ios-app/
├── package.json              npm設定（Capacitor + プラグイン）
├── capacitor.config.json     アプリID・名前・プラグイン設定
├── sync-web.sh               ../のWebアプリを www/ にコピー
├── assets/                   アイコン元画像（1024px）
├── www/                      同梱するWeb資産（自動生成・git管理外）
└── ios/                      Xcodeプロジェクト
    └── App/App.xcworkspace   ← これを開く
```

## よく使うコマンド

```bash
npm install        # 依存取得（初回のみ）
npm run sync       # ../index.html の変更をiOSプロジェクトへ反映
npm run icons      # アイコン・スプラッシュ再生成
npm run open       # Xcodeで開く（要Xcode）
```

## アプリ本体を修正したら

1. `../index.html` を編集
2. `npm run sync` を実行
3. Xcodeで再ビルド

Web版（GitHub Pages）はリポジトリのルートをそのまま配信しているので、
`../index.html` を push すればWeb版も同時に更新されます。

## ネイティブ専用の動作

`../index.html` 内で `NATIVE`（Capacitor検出）により分岐しています。

| 機能 | Web版 | ネイティブ版 |
|------|-------|-------------|
| 触覚 | `navigator.vibrate` | Capacitor Haptics（本物の触覚） |
| リマインド通知 | アプリを開いている時のみ | **閉じていても定時に届く**（ローカル通知） |

## メモ

- `npm audit` の警告はアイコン生成ツールの依存によるもので、アプリ本体には含まれません。
- `Pods/` `node_modules/` `www/` はgit管理外です。
