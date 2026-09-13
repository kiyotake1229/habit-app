# コツコツ — 習慣＆タスク

続けるほど「称号」が育ち、サボると錆びる習慣管理アプリ。

| 項目 | 内容 |
|---|---|
| 状態 | **iOS 申請可能**（Web版・PWA版は完成） |
| Web公開 | https://kiyotake1229.github.io/habit-app/ |
| 本体 | `index.html`（約96KB） |
| 通信 | なし（完全オフライン） |
| データ | 端末内のみ（localStorage＋ネイティブ保存の二重化） |
| PWA | 対応（`manifest.json` / `sw.js` / アイコン一式） |
| iOS | **Capacitor 構築済み** → [`ios-app/`](ios-app/) |

---

## 岩崎さんへ

App Store への申請手順は **[ios-app/岩崎さんへの引き渡し手順.md](ios-app/岩崎さんへの引き渡し手順.md)** にまとめてあります。
申請時に入力する文面（説明文・キーワードなど）も下書き済みです。

---

## 主な機能

- **称号が育つ** — 習慣を続けると「読書の常連」「運動の達人」のように称号が進化する
- **サボると錆びる** — やらない日が続くと称号が錆び、5日空くと1段階下がる。今日やれば戻る
- **月1回の休息チケット** — サボり扱いにならず、ストリークを守れる
- **称号カードの共有** — 画像にしてSNSへ
- 回数で数える習慣（水を8杯など）、週N回の習慣、過去日のあとから記録
- レベルアップ、12種類の実績バッジ、週間グラフ、カレンダー
- 一時停止（旅行・体調不良時）、定時リマインド通知、ダークモード、バックアップ/復元

## ネイティブ版の追加機能

| 機能 | Web版 | iOS版 |
|---|---|---|
| 触覚フィードバック | `navigator.vibrate` | 本物の触覚 |
| リマインド通知 | アプリを開いている時のみ | **閉じていても定時に届く** |
| データ保存 | localStorage | localStorage ＋ ネイティブ保存（消失対策） |

## ファイル構成

```
habit/
  index.html            アプリ本体
  manifest.json / sw.js PWA用
  icon.svg              アイコン元データ
  icon-192.png / icon-512.png / apple-touch-icon.png
  ios-app/              iOSプロジェクト（Capacitor）
    岩崎さんへの引き渡し手順.md
    README.md           開発者向けメモ
    v1.1-HealthKit連携メモ.md   次バージョンの技術調査（未着手）
```

## 修正したいとき

`index.html` を直す → `ios-app/` で `npm run sync` → Xcode で再ビルド。
Web版は `index.html` を GitHub に push すれば同時に更新される。

## 次バージョンの構想

**v1.1: HealthKit連携（歩数・運動量）** — 技術調査済み。v1.0 の審査通過後に着手する方針。詳細は [ios-app/v1.1-HealthKit連携メモ.md](ios-app/v1.1-HealthKit連携メモ.md)。
