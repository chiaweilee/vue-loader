<i18n lang="yaml">
en:
  hello: "hello world"
ja:
  hello: "こんにちは、世界"
</i18n>

<blog>## foo</blog>

<esm>
export default function (Component) {
  Component.options.foo = 1
}
</esm>

<template>
  <div>
    <h1>{{ msg }}</h1>
    <p v-html="blog"></p>
  </div>
</template>
