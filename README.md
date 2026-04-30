# 🎵 CrossfadePlayer

ブラウザ上で動作する音声クロスフェーダーアプリケーション。2つの音源をURLから読み込み、同時再生しながらクロスフェーダーでリアルタイムに音量バランスを制御できます。

**▶ [アプリを開く](https://kagoiri.github.io/CrossfadePlayer/)**

## 特徴

- **Web Audio API** による高品質な音声再生・制御
- **クロスフェーダー** で2つの音源の音量バランスをリアルタイムに変更（等パワーカーブ対応）
- **個別音量調整** で各トラックの音量を独立して制御
- **同期再生** — 2つの音源の再生位置を常時同期
- **波形表示** — 各トラックの波形をCanvasで描画、クリックでシーク
- **Google Drive / Dropbox対応** — 共有リンクをそのまま貼り付けて読み込み可能
- **URL共有** — 音源アドレス込みのURLを生成し、他の人と同じ設定を共有
- **キーボードショートカット** — スペースキーで再生/停止、矢印キーでクロスフェーダー操作
- **ループ再生** — 繰り返し聴きながらクロスフェードを調整
- **レスポンシブデザイン** — デスクトップ・モバイル両対応
- **単一ファイル構成** — `index.html` のみで動作、外部依存なし

## 対応フォーマット

WAV / MP3 / FLAC / OGG

## 使い方

### 基本操作

1. Track A と Track B にそれぞれ音源のURLを入力
2. 「読み込み」ボタンをクリック
3. 両方の音源が読み込まれたら「▶ 再生」で同期再生開始
4. クロスフェーダーを左右に動かして音量バランスを調整

### Google Drive / Dropbox の音源を使う

Google Drive や Dropbox の共有リンクをそのまま URL 入力欄に貼り付けてください。自動的に直接ダウンロード用のURLに変換されます。

- **Dropbox**: 共有リンクをそのままコピーしてください（追加設定不要）
- **Google Drive**: CORSプロキシの設定が必要です（下記参照）

### Google Drive 用 CORSプロキシの設定

Google Drive はブラウザからの直接ダウンロードをセキュリティポリシーでブロックするため、CORSプロキシが必要です。Cloudflare Workers（無料）で簡単にセットアップできます。

1. [Cloudflare Workers](https://workers.cloudflare.com/) でアカウント作成
2. Workers & Pages → Create → Create Worker
3. リポジトリ内の `cors-proxy-worker.js` のコードを貼り付けて Deploy
4. 発行されたURL（例: `https://my-proxy.username.workers.dev`）をコピー
5. CrossfadePlayer の画面下部「⚙ 設定」→「CORSプロキシURL」に貼り付けて保存

設定は localStorage に保存されるため、次回以降は自動的に使用されます。

### URL共有

「🔗 共有用URLをコピー」ボタンをクリックすると、現在読み込んでいる音源の組み合わせを含むURLがクリップボードにコピーされます。このURLを他の人に送ると、同じ音源の組み合わせでCrossfadePlayerを開けます。

### キーボードショートカット

| キー | 操作 |
|------|------|
| `Space` | 再生 / 停止 |
| `←` | クロスフェーダーを左へ（Track A寄り） |
| `→` | クロスフェーダーを右へ（Track B寄り） |

## デプロイ

`index.html` 1ファイルで完結しているため、任意の静的ホスティングサービスに配置するだけで公開できます。

### GitHub Pages

1. このリポジトリの Settings → Pages を開く
2. Source を `main` ブランチ、`/ (root)` に設定
3. `https://<ユーザー名>.github.io/CrossfadePlayer/` で公開

### その他のホスティング

Netlify、Cloudflare Pages、Vercel などでも同様に `index.html` を配置するだけで動作します。

> **注意**: Web Audio API は HTTPS 環境でのみ動作します。上記サービスはすべて HTTPS に対応しています。

## 開発

### セットアップ

```bash
npm install
```

### テスト実行

```bash
npm test
```

Vitest + fast-check によるユニットテスト・プロパティベーステストが実行されます。

### プロジェクト構成

```
CrossfadePlayer/
├── index.html              # アプリケーション本体（HTML/CSS/JS統合）
├── src/                    # テスト用モジュール（個別ファイル）
│   ├── audio-engine.js     # Web Audio API 音声再生・制御
│   ├── audio-math.js       # クロスフェード計算・音量計算
│   ├── keyboard-handler.js # キーボードショートカット
│   ├── share-manager.js    # URL共有管理
│   ├── url-converter.js    # Google Drive/Dropbox URL変換
│   └── waveform-renderer.js # 波形描画
├── tests/                  # テストファイル
├── package.json
└── vitest.config.js
```

`src/` 内のモジュールは `index.html` にインライン化されています。`src/` はテスト実行用に保持されています。

## 技術スタック

| 技術 | 用途 |
|------|------|
| HTML5 / CSS3 / Vanilla JS | アプリケーション全体 |
| Web Audio API | 音声再生・制御 |
| Canvas API | 波形描画 |
| History API | URL共有 |
| Vitest + fast-check | テスト |

## ライセンス

MIT
