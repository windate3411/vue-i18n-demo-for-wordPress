<template>
  <div id="app">
    <h1>{{ $t("message", { msg: "World!" }) }}</h1>
    <h2>My name is {{ $t("name") }}</h2>
    <h2>And my hobbiees are {{ $t("hobby") }}</h2>
    <h2>I have {{ $tc("brothers", 0) }}</h2>
    <h2>I have {{ $tc("brothers", 1) }}</h2>
    <h2>I have {{ $tc("brothers", 2, { count: 2 }) }}</h2>
    <select v-model="$i18n.locale">
      <option v-for="(lang, i) in langs" :key="`lang${i}`" :value="lang">{{
        lang
      }}</option>
    </select>
  </div>
</template>

<script>
export default {
  data() {
    return {
      langs: ["tw", "en"],
    }
  },
}
</script>
