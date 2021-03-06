# スコープ付き CSS

**再帰されたコンポーネントの子孫セレクタには気をつけてください！** セレクタ `.a .b` を持つ CSS ルールの場合、`.a` にマッチする要素に再帰的な子コンポーネントが含まれている場合、その子コンポーネントのすべての `.b` はルールにマッチします。# スコープ付き CSS

`scoped` 属性をもつ `<style>` タグを利用するとき、その CSS は現在のコンポーネントの要素にのみ適用されます。これは Shadow DOM のスタイルのカプセル化に似ています。いくつか注意点がありますが、polyfill は必要ありません。PostCSS を使用して以下を変換することによって実現しています:

``` html
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

以下の通りになります:

``` html
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

## 秘訣

### ローカルスタイルとグローバルスタイルの混在

同じコンポーネントでスコープ付き、そして非スコープ付きスタイル両方を含むことができます:

``` html
<style>
/* グローバルスタイル */
</style>

<style scoped>
/* ローカルスタイル */
</style>
```

### 子コンポーネントのルート要素

`scoped` によって、親コンポーネントのスタイルは子コンポーネントに漏れません。ただし、子コンポーネントのルートノードは親スコープの CSS と子スコープの CSS と両方によって影響を受けます。これは、設計上、親はレイアウトが目的で子のルート要素をスタイルすることができます。

### ディープセレクタ

`scoped` スタイルのセレクタを "deep" にしたい、つまり子コンポーネントに及ぼしたい場合は、`>>>` コンビネータを使用することができます:

``` html
<style scoped>
.a >>> .b { /* ... */ }
</style>
```

上記は以下にコンパイルされます:

``` css
.a[data-v-f3f3eg9] .b { /* ... */ }
```

SASS のようないくつかのプリプロセッサは、`>>>` を適切に解析できないかもしれません。そのようなケースでは `/deep/` コンビネータを代わりに使用することができます。それは、`>>>` のエイリアスで、全く同じような動作します。

### 動的に生成されるコンテンツ

`v-html` によって作成された DOM コンテンツは、スコープ付きスタイルにの影響を受けませんが、ディープセレクタを使用してスタイルを設定することはできます。

### 留意すること

- **スコープ付きスタイルでは class は必要です。** ブラウザが様々な CSS セレクタを描画するため、`p { color: red }` はスコープされているとき何倍も遅くなります（すなわち属性セレクタと組み合わせた場合）。もし `.example { color: red }` のように class か id を使用するなら、パフォーマンスヒットは事実上なくなります。[この例](https://stevesouders.com/efws/css-selectors/csscreate.php)で違いをテストすることが出来ます。
- **再帰されたコンポーネントの子孫セレクタには気をつけてください！** セレクタ `.a .b` を持つ CSS ルールの場合、`.a` にマッチする要素に再帰的な子コンポーネントが含まれている場合、その子コンポーネントのすべての `.b` はルールにマッチします。
