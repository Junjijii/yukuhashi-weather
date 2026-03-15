# プロジェクトステータス

## 概要
福岡県行橋市のリアルタイム天気 + 雨雲レーダーを表示するデスクトップウィジェット。
HTMLファイル1枚で動作し、ブラウザで開くだけで使える。

## 現在のバージョン / 状態
v0.1.0 - `index.html` 実装完了、動作確認中

## 協業ステータス
- lead: Claude Code
- executor: Codex
- phase: codex-complete-awaiting-claude
- handoff_ready: false
- next_owner: Claude Code
- final_owner: Claude Code
- updated_at: 2026-03-15 15:15 JST

## 設計（Codex向け実装指示）

### 技術スタック
- HTML + CSS + JavaScript（単一ファイル `index.html`）
- 天気API: [Open-Meteo](https://open-meteo.com/)（無料・APIキー不要）
- 雨雲レーダー: [RainViewer API](https://www.rainviewer.com/api.html)（無料・APIキー不要）
- 地図: [Leaflet.js](https://leafletjs.com/) v1.9.4（CDN読み込み）
- 地図タイル: CartoDB Dark Matter

### 行橋市の座標
- 緯度: 33.7289
- 経度: 130.9833

### 機能要件
1. **現在の天気表示**
   - 気温（大きく表示）
   - 天気アイコン（絵文字）+ 天気の説明（日本語）
   - 湿度・風速・風向
2. **5日間の予報**
   - 曜日 + 天気アイコン + 最高/最低気温
3. **雨雲レーダー**
   - Leaflet地図上にRainViewerのレーダータイルをオーバーレイ
   - 行橋市中心、ズーム8程度
   - 再生ボタン（▶/⏸）でアニメーション再生（過去〜予測）
   - スライダーで任意の時間を手動選択
   - 雨の強さの凡例（弱→強のカラーバー）
4. **自動更新**
   - 天気: 10分ごと
   - レーダー: 5分ごと

### デザイン要件
- macOS風フロストガラスUI（`backdrop-filter: blur`）
- ダークテーマ（背景 `rgba(30,30,40,0.85)` 系）
- 角丸24px、白文字、半透明ボーダー
- ウィジェット幅: 380px程度
- フォント: `-apple-system, BlinkMacSystemFont, 'Hiragino Sans'`
- レーダー地図の高さ: 240px、角丸14px
- Leafletのattributionコントロールは非表示

### ファイル構成
```
index.html  ← これ1つだけ
```

### API仕様メモ
- Open-Meteo: `https://api.open-meteo.com/v1/forecast?latitude=33.7289&longitude=130.9833&current=temperature_2m,relative_humidity_2m,weather_code,wind_speed_10m,wind_direction_10m&daily=weather_code,temperature_2m_max,temperature_2m_min&timezone=Asia/Tokyo&forecast_days=6`
- RainViewer: `https://api.rainviewer.com/public/weather-maps.json` でフレーム一覧取得 → `{host}{path}/256/{z}/{x}/{y}/2/1_1.png` でタイル取得

## 直近の変更（最新を上に追記）
| 日付 | 変更内容 | 担当 |
|------|---------|------|
| 2026-03-15 | Codex の実装ターン完了。Claude Code 向け引き継ぎメモを追加し、レビュー待ち状態へ更新 | Codex |
| 2026-03-15 | `index.html` に現在天気、5日間予報、RainViewer レーダー、再生ボタン、スライダー、凡例、自動更新を実装 | Codex |
| 2026-03-15 | 設計完了、Issue作成、handoff_ready: true | Claude Code |

## 次にやること
- [ ] ブラウザで `index.html` を開いた目視確認
- [ ] 必要に応じてレーダーアニメーション速度やズームを微調整

## 現在の問題
なし

## 引き継ぎメモ
- from: Codex
- to: Claude Code
- branch: `codex/issue-1-2-weather-radar`
- commit: `19a738e`
- summary: `index.html` を単一ファイルで実装。現在天気、5日間予報、RainViewer レーダー、再生/停止、スライダー、凡例、自動更新を追加済み。最終統合とユーザーへの返答は Claude Code 側で回収する前提。次はブラウザでの目視確認と必要なら微調整。
- tests: `html.parser` での静的パース通過。Open-Meteo / RainViewer のライブ API 応答確認済み。ブラウザ目視は未実施。

## ファイル構成
- `index.html` - ウィジェット本体
- `PROJECT_STATUS.md` - プロジェクト状態管理
- `CLAUDE.md` - Claude Code用ガイド
- `AGENTS.md` - Codex用ガイド

## テスト方法
- `index.html` をブラウザで開いて目視確認
- 天気データが表示されること
- レーダー地図が表示され、再生・スライダーが動作すること
- 静的確認: `python3` の `html.parser` で `index.html` をパース

## デプロイ / リリース方法
- `index.html` をダブルクリックでブラウザで開く
