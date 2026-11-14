# contrast.css

純白 ⇄ 純黒のコントラストだけで組む、依存ゼロ・フレームワーク不問の単体CSSライブラリ。ポートフォリオ・資料系Webアプリ用の「コントラストテンプレート」から抽出した、33個の `.ct-*` クラス。

- 色は `#FFFFFF` / `#000000` のみ(階調は白黒の不透明度のみ)。例外色はエラー赤 `#DC2626` のみ
- 角丸なし・影なし・グラデーションなし(`border-radius: 0` / `box-shadow: none` を明示)
- 装飾は罫線と反転のみ。強調は色ではなく**反転**で行う
- Tailwind等の依存なし。1ファイルを読み込むだけで動く

## インストール

```sh
npm install contrast-css
```

CDN(公開後):

```html
<link rel="stylesheet" href="https://unpkg.com/contrast-css@latest/contrast.css">
```

## 使い方

```html
<link rel="stylesheet" href="node_modules/contrast-css/contrast.css">
```

紙面を `<section class="ct-paper-white">` と `<section class="ct-paper-black">` で交互に敷くと、トークンが紙面ごとに反転します。黒紙面では反転ブロック(`.ct-em` など)が自動的に白地黒字になります。

```html
<section class="ct-paper-white">
  <div class="ct-container">
    <p class="ct-label">LABEL</p>
    <h2 class="ct-h2">大見出し<span class="ct-em">反転強調</span></h2>
    <p class="ct-sub">階調は白黒の不透明度のみ。</p>
  </div>
</section>

<section class="ct-paper-black">
  <div class="ct-container">
    <div class="ct-rule"></div>
    <ul class="ct-rows">
      <li>上下太罫の1行リスト</li>
    </ul>
  </div>
</section>
```

## クラス一覧

| クラス | 役割 |
| --- | --- |
| `.ct-paper-white` / `.ct-paper-black` | 紙面トークン(背景・文字・罫・反転を一括定義) |
| `.ct-container` | 中央寄せコンテナ |
| `.ct-section` | セクション縦幅 |
| `.ct-grid-2` / `.ct-grid-4` | 2段 / 4段グリッド |
| `.ct-h1` / `.ct-h2` / `.ct-h3` | 見出し(serif極太・追い込み) |
| `.ct-label` | ラベル(mono・字間広) |
| `.ct-mono` | 等幅 |
| `.ct-ruby` | ルビ |
| `.ct-sub` | 副本文(不透明度0.6) |
| `.ct-rule` | 太罫(4px) |
| `.ct-frame` | 額縁(4px) |
| `.ct-outline` | 小枠(2px) |
| `.ct-rows` | 上下太罫の1行リスト |
| `.ct-pitch` | 左太罫ピッチ |
| `.ct-lined` | 細罫連続リスト |
| `.ct-em` | 反転強調スパン |
| `.ct-band` | タグライン帯 |
| `.ct-chip` / `.ct-badge` | チップ・バッジ(反転) |
| `.ct-pairs` | 対立ペアのリスト |
| `.ct-facts` | FACTS定義リスト |
| `.ct-table` | 枠付き表 |
| `.ct-input` / `.ct-select` / `.ct-textarea` | フォーム部品 |
| `.ct-btn` / `.ct-btn-solid` | ボタン(枠 / 反転) |
| `.ct-error` | エラー(唯一の例外色) |
| `.ct-reveal` | 出現演出(`.is-shown` で表示) |

## 出現演出(省略JS)

`.ct-reveal` はCSSのみでは動かないので、必要なら次のスニペットを `<script>` に写します。しきい値0.15・一度きりで、`prefers-reduced-motion` にもCSS側で対応済みです。

```js
const io = new IntersectionObserver(function (entries) {
  for (const e of entries) {
    if (e.isIntersecting) {
      e.target.classList.add("is-shown");
      io.unobserve(e.target);
    }
  }
}, { threshold: 0.15 });
document.querySelectorAll(".ct-reveal").forEach(function (el) {
  io.observe(el);
});
```

## Tailwind対照(設計出典)

このライブラリはTailwind CSS製テンプレートから抽出しており、主要クラスの元ユーティリティは次の通りです。

| contrast.css | 元Tailwindユーティリティ |
| --- | --- |
| `.ct-paper-white` / `.ct-paper-black` | `bg-white text-black` / `bg-black text-white` |
| `.ct-container` | `max-w-4xl mx-auto px-6` |
| `.ct-section` | `py-16 md:py-24` |
| `.ct-h1` | `font-serif font-black leading-none tracking-tight text-6xl md:text-9xl` |
| `.ct-h2` | 同上 `text-4xl md:text-6xl` |
| `.ct-h3` | 同上 `text-3xl md:text-5xl` |
| `.ct-label` | `font-mono text-xs tracking-widest` |
| `.ct-sub` | `text-black/60` / `text-white/60` |
| `.ct-em` | `bg-black text-white px-2`(黒紙面では白地黒字) |
| `.ct-band` | `bg-black text-white px-4 py-2 font-bold` |
| `.ct-frame` | `border-4` |
| `.ct-outline` | `border-2` |
| `.ct-rule` | `border-b-4` |
| `.ct-rows` | `border-y-4 divide-y-2` |
| `.ct-pitch` | `border-l-4 pl-4` |
| `.ct-input` 等 | `border-2 px-3 py-2` |
| `.ct-error` | `text-red-600 font-bold` |

## 規約

- 紙面は純白 ⇄ 純黒の交互。色は `#FFF` / `#000` のみ
- 階調は白黒の不透明度のみ。例外色はエラー赤 `#DC2626` のみ
- 角丸なし(`border-radius: 0`)。影なし。太罫4px・細罫2px
- 大見出しは serif 極太、ラベル・数値は mono
- 無限アニメーション禁止(出現は一度きりのフェード `.ct-reveal` のみ)

## License

MIT