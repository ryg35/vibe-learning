# vibe-learning

新しいテーマを2時間で「使える」ところまで持っていくための Claude Code skill。
AIに「教えて」と聞くのではなく、**教材そのものをAIに作らせる。**

生成されるのは、ブラウザでそのまま開ける1ファイルのHTML教材。タイマー、所要時間つきの目次、
演習、クイズ、進捗バーが入っていて、最後に自分の成果物をMarkdownで持ち帰る。

実際に生成した教材:

- [SPEC駆動開発を、2時間で「使える」ところまで](https://ryogohosoi.com/learn/spec-driven-development) ... 全8章120分
- [UIコンポーネント見本帳](https://ryogohosoi.com/learn/uiux-component-catalog) ... 24パターンを動くサンプルで

## インストール

```bash
git clone https://github.com/ryg35/vibe-learning.git ~/.claude/skills/vibe-learning
```

zip で落としたい場合は [ryogohosoi.com/learn](https://ryogohosoi.com/learn) から。

## 使い方

Claude Code で、学びたいことを言うだけ。

```
SPEC駆動開発を2時間で学びたい
```

または `/vibe-learning` で明示的に呼ぶ。

## 何をするか

```
Step 0  テーマの粒度を決める        （対話）
   ↓
Step 1  Research プロンプトを渡す    ★外注。Claudeは検索しない
   ↓    ChatGPT等に投げて、結果を貼り戻す
   ↓
Step 2  教材HTML を生成する
   ↓
Step 3  保存して開く
```

### なぜ Research を外注するのか

Claude 自身に検索させない。理由は2つ。

- 検索に強いモデル（ChatGPTのDeep Research等）と、長い文書の生成に強いモデル（Claude）で
  担当を分けたほうが、どちらの仕事も丁寧になる
- リサーチ結果を Claude のコンテキストに溜め込むと、教材生成に使える余力が削られる。
  Step 2 は数万トークンのHTMLを一息に書く工程で、そこが本番

リサーチと教材化を1回でやらせると、どこかの記事をまとめ直したような、ふわっとした説明が
返ってくる。情報源のURLも曖昧になり、教材の各セクションに参考リンクを付けられなくなる。

### テーマの粒度

2時間で触れる粒度に絞る。

- 広すぎる: 「機械学習」「Web開発」「マーケティング」
- ちょうどいい: 「SPEC駆動開発」「UIコンポーネントの名前」「プロジェクトリーダーの仕事」

粒度が広いまま走ると、目次だけ立派で中身が薄い教材ができる。
2時間を使ったあとで気づくので、損失が大きい。

## 中身

| ファイル | 何が書いてあるか |
|---|---|
| `SKILL.md` | ワークフロー本体。絶対規則は2つだけ |
| `references/research-prompt.md` | Step 1 のリサーチプロンプト全文 |
| `references/teaching-prompt.md` | Step 2 の教材生成プロンプト全文 |
| `references/material-design.md` | 教材HTMLのデザイン規範。トークン、部品、出力前チェック |

プロンプト2本は全文そのまま置いてある。skill を入れずに、コピーして使ってもいい。

## 技術テーマ専用ではない

一次情報が存在するテーマなら同じ2本で回る。実績:

- SPEC駆動開発（開発手法）
- UIコンポーネント見本帳（デザイン語彙）
- プロジェクトリーダーの仕事（非技術）

回らないのは一次情報が存在しない領域（個人の経験則、社内固有の事情）で、
そこは Research の出力が薄くなるのですぐ分かる。

## ライセンス

MIT
