# 教材HTMLのデザイン規範

Notion の Design System を下敷きにした、2時間集中学習用の教材UI。
原本は `https://ryogohosoi.com/learn/spec-driven-development`。ブラウザで開いて、
ソースを読める（1ファイル完結なので、ページのソースがそのまま実装の全部）。

**迷ったら原本を開いて該当箇所を読む。** ここに書いてあるのは骨格であって、全部ではない。

## 大原則

- **1ファイル完結。** HTML / CSS / JavaScript を1つの `.html` に収める
- **外部リソースをゼロにする。** CDN・Webフォント・外部画像・外部スクリプトを読み込まない。オフラインで開けること、および配信先のCSPを通すため（`<a href>` の本文リンクは例外。これは踏むためのリンクなので外部で構わない）
- **装飾しすぎない。** 2時間集中するための実用品であって、作品ではない
- **絵文字はアイコンとして使う。** 画像を読み込まずに視覚的な手がかりを足せる。Callout の先頭に1つだけ

## トークン（そのまま流用してよい）

```css
:root{
  --text:#37352F;
  --text-2:rgba(55,53,47,.65);
  --text-3:rgba(55,53,47,.45);
  --line:rgba(55,53,47,.09);
  --line-2:rgba(55,53,47,.16);
  --bg:#ffffff;
  --bg-hover:rgba(55,53,47,.05);
  --code-bg:rgba(135,131,120,.15);
  --code-text:#EB5757;
  --blue:#E7F3F8;   --blue-t:#183347;
  --gray:#F1F1EF;
  --yellow:#FBF3DB; --yellow-t:#402C1B;
  --red:#FDEBEC;    --red-t:#5D1715;
  --green:#EDF3EC;  --green-t:#1C3829;
  --purple:#F6F3F9; --purple-t:#412454;
  --accent:#2383E2;
  --font: -apple-system, BlinkMacSystemFont, "Segoe UI", "Hiragino Sans",
          "Hiragino Kaku Gothic ProN", "Yu Gothic UI", Meiryo, sans-serif;
  --mono: "SFMono-Regular", ui-monospace, Menlo, Consolas,
          "Liberation Mono", "Courier New", monospace;
}
body{ font-size:16px; line-height:1.75; }
```

`--font` はOS標準フォントだけで組んである。Webフォントを足さないための指定なので、変えない。

## レイアウト

```
.layout (display:flex; max-width:1240px; margin:0 auto)
├── aside   position:sticky; top:0  … タイマー / 進捗 / 目次
└── main                              … 本文
@media(max-width:1000px) で aside を畳む
```

`html{ scroll-behavior:smooth; scroll-padding-top:80px; }` を必ず入れる。
目次から飛んだとき、見出しが画面上端に貼りつかない。

## サイドバーの3点セット

学習を完走させるための装置。**3つとも入れる。**

### 1. タイマー（`.side-box`）

```
00:00:00        .timer-val   （--mono / 22px / tabular）
開始するとタイマーが動きます   .timer-sub
[開始] [リセット]  .btn / .btn-primary
```

`setInterval` で 1 秒ごとに再描画するだけ。永続化はしない。

### 2. 進捗バー（`.side-progress`）

```
進捗                    0 / 28
▓▓▓▓░░░░░░░░░░░░░░░░
```

本文中の `input[type=checkbox]` を全部数え、チェック済み数を分子にする。
**チェックボックスの総数が、学習者から見た「残り作業量」になる。** ここが多すぎると
最初の画面で心が折れるので、章あたり 3〜5 個に収める。

### 3. 目次（`.toc`）

```
1  SDDの基本思想            10分
2  全体フローと品質ゲート     10分
```

`.num` / タイトル / `.min`（所要分）の3カラム。スクロール位置に応じて `.active` を付け替える。
**各項目に所要時間を必ず出す。** 合計が宣言した2時間に一致していること。

## 本文の部品

### セクションヘッダ（`.sec-head`）

```html
<div class="sec-head">
  <span class="time-chip">10分</span>
</div>
<h2>1. SDDの基本思想</h2>
<p class="goal">このセクションのゴールを1文で書く</p>
```

`border-top: 1px solid var(--line)` で章の切れ目を作る。`.goal` は必須。
「この章を終えると何ができるか」を1文で書く。書けないなら、その章は要らない。

### Callout（`.callout` + 色クラス）

```html
<div class="callout c-blue">
  <span class="ico">🧭</span>
  <div class="body">本文</div>
</div>
```

| クラス | 用途 |
|---|---|
| `c-blue` | 使い方の案内、前提の共有 |
| `c-yellow` | 注意、つまずきやすい点 |
| `c-red` | やってはいけないこと |
| `c-green` | できるようになったことの確認 |
| `c-gray` / `c-purple` | 補足、余談 |

**1章に2つまで。** 全部Calloutにすると、どれも目立たなくなる。

### 演習（`.ex` + `.b-ex`）とクイズ（`.ex` + `.b-quiz`）

```html
<div class="ex">
  <div class="ex-head"><span class="ex-badge b-ex">演習</span><h4>見出し</h4></div>
  <p class="ta-label">ここに書く</p>
  <textarea></textarea>
</div>
```

クイズは `.opts` に `.opt` ボタンを並べ、押したら `.correct` / `.wrong` を付け、
`.q-fb.show` で解説を出す。**解説は正解時にも出す。** 当てずっぽうで当たった人を素通りさせない。

演習の入力欄は空欄のままでも進めてしまうので、教材冒頭の使い方Calloutで
「必ず自分の言葉で埋める。空欄だと最後の成果物が作れない」と明示する。

### フロー図（`.flow`）

```html
<div class="flow">
  <span class="step">Specify</span><span class="arw">→</span>
  <span class="step gate">レビュー</span><span class="arw">→</span>
  <span class="step">Implement</span>
</div>
```

画像を使わずに手順を図にする方法。`.gate` は品質ゲート（黄色）。

### 出典（`.src`）

**各セクションの末尾に必ず置く。** リサーチで渡された URL のうち、その章の根拠になったものだけ。

```html
<p class="src"><b>参考:</b> <a href="...">ページタイトル</a></p>
```

これがない教材は、あとから検算できない。書けないセクションは、根拠なしで書いている。

## 成果物の持ち帰り

最後の章で、演習の入力を集めて Markdown を組み立て、`Blob` + `a.download` で保存させる。
**入力内容はブラウザに保存されない。** その事実を冒頭の使い方Calloutに明記する
（`localStorage` を使わないのは、教材を配信先で共有したときに他人の入力が混ざらないようにするため）。

コピー用ボタンを併設するなら `navigator.clipboard` を使い、失敗時は
`document.execCommand('copy')` に落とす。`file://` で開かれる可能性があるため。

## 冒頭に必ず置くもの

```
eyebrow      2時間 VIBE LEARNING
h1           <テーマ>を、2時間で「使える」ところまで
.lead        何が身につくか。読むだけで終わらせないこと。最後に何を持ち帰るか
.meta        [所要 120分] [実践 65分 / 講義 55分] [前提: ...] [情報は YYYY年M月D日時点]
callout      この教材の使い方（4項目 + 保存されない旨の注記）
table        2時間のタイムテーブル（時刻 / 分 / やること / 種類）
```

`.meta` の**日付タグは必ず入れる。** 教材は生成時点のリサーチで固まっており、
半年後に読む自分がそれを知らないと事故る。

タイムテーブルの「種類」列は 理解 / 実践 の2値。**実践の合計が講義の合計を下回らないこと。**
下回っているなら、それは教材ではなく記事になっている。

## 出力前のチェック

- [ ] 外部リソースの読み込みがゼロ（`<link rel=stylesheet>` / `<script src>` / `<img src=http` を grep して確認）
- [ ] 目次の所要時間の合計 = 宣言した2時間
- [ ] 実践の合計時間 >= 講義の合計時間
- [ ] 全セクションに `.goal` と `.src` がある
- [ ] チェックボックスが章あたり3〜5個
- [ ] 最後の章で成果物がダウンロードできる
- [ ] `.meta` に情報の日付が入っている
- [ ] リサーチ結果になかった事実を書いていない
