# vue-i18n-demo

## Description

這是一個為了介紹 vue-i18n 而打造的示範專案，完整的文章內容如下

# wordpress 在你的專案中導入多國語系! 簡單介紹 Vue-i18n 基礎使用!

## 前言

多國語言的需求在現代網頁開發已經是個越來越常見的要求，值得慶幸的是現在有各種的 plugin 讓我們能比較輕鬆地達到想要的效果，今天就要針對如何在你的 vue 專案中加入多語系的效果! 別擔心，真的相當容易的!

## 專案需求

本次 demo 我會採用 vue-cli 來打造示範專案，原因是較為方便! 所以如果想跟著 demo 一起操作的話，你的環境需要有以下的事前配置。

- [vue-cli](https://cli.vuejs.org/)
- [node.js](https://nodejs.org/en/)

## 什麼是 vue-i18n?

vue-i18n 是一個可以將你的 app 國際化的套件，我沒開玩笑，原文就是這樣寫的(
Vue I18n is internationalization plugin for Vue.js)。藉由導入 vue-i18n，你頁面上的內容會按照你預設的顯示語言呈現，並在特定的情況轉為其他語系。

## 安裝 vue-i18n

首先我們先利用 vue-cli 快速搭建專案，請在你的終端機輸入以下的內容

```
vue create vue-i18n-demo
```

並在設定的部分選擇 default 即可

![setting](https://i.imgur.com/t2lmuoF.png)

待安裝完成後，輸入以下指令運行我們的專案。
根據指示進入http://localhost:8080/後
你應該會看到那個熟悉的 vue 官方文件畫面!

```
cd vue-i18n-demo
npm run serve
```

![vue-cli start image](https://i.imgur.com/TR3SZtI.png)

接著回到終端機，輸入以下指令安裝 vue-i18n

```
vue add i18n
```

接下來你會看到一連串的問題，我們一個一個來

- The locale of project localization.

這個專案的預設語系，**請輸入 tw**。

- The fallback locale of project localization.

這個專案中若沒有找到預設語系的翻譯檔案時，要以哪個語系的翻譯檔作為顯示，**這邊請輸入 en**。

- The directory where store localization messages of project. It's stored under `src` directory.

專案中翻譯檔的位置，這邊我們採用預設值，**請直接按下 Enter**

- Enable locale messages in Single file components ?

是否開放在每個 vue 檔案中夾帶翻譯檔，我個人習慣集中管理，**這邊請輸入 N**

到此為止整個基本設定就完成了，你會發現專案中多了幾個檔案，我們先看一下我們剛剛做的設定，在根目錄下應該會有個 vue.config.js 檔案有著以下內容

```
module.exports = {
  pluginOptions: {
    i18n: {
      locale: 'tw',
      fallbackLocale: 'en',
      localeDir: 'locales',
      enableInSFC: false
    }
  }
}
```

且在 src/locales 底下有兩個 json 檔案，這些是我們之後要新增翻譯內容時要修改的檔案。

| ![project structure](https://i.imgur.com/VeFS5te.png) |
| :---------------------------------------------------: |


## vue-i18n 功能探索

### 前置作業

現在我們馬上來體驗一下一些 i18n 的基礎功能吧，首先我們先將 App.vue 的內容清空，並改為以下的內容。

```
// src/App.vue
<template>
  <div id="app">
    <h1>{{ $t("message") }}</h1>
    <h2>My name is {{ $t("name") }}</h2>
    <h2>And my hobbiees are {{ $t("hobby") }}</h2>
  </div>
</template>

<script>
export default {

}
</script>
```

接著為了之後的示範，我們要修改一下一開始產出的兩個翻譯檔案，請將 locales 下方的兩個 json 檔案改為以下的內容，也許你會好奇為什麼兩個檔案的內容不太相同，這是為了之後的 demo，暫時先不用擔心!

![json-files](https://i.imgur.com/3ckSVRc.png)

最終你應該會看到以下的畫面

![pre-demo](https://i.imgur.com/urE7hkg.png)

### 基礎使用

在 vue 元件內，我們可以用兩種方法使用我們在 json 檔案中的翻譯資料
當你加入 i18n plugin 後，你的 vue 其實會被多綁一個\$t 物件，其中包含著所有你建立的翻譯字元，理解這一點之後剩下的就好辦囉!

- **在 template 中利用 mustache syntax**

下方的寫法最終會在 locales 中的 json 檔案找到 message 對應的值

```
<template>
  <div id="app">
    <h1>{{ $t("message") }}</h1>
  </div>
</template>
```

- **在 script 中利用 this.\$t 存取**

有時候你也會碰到需要在 script 中操作的情況，這時候你可以利用類似以下的方法操作 message 值

```
methods: {
    greet() {
      alert(this.$t("message"))
    }
}
```

### 在訊息中加入變數

在翻譯的文字中有時候會需要加入變數操作，這也可以輕易地透過 i18n 操作，舉個例子來說，今天我們希望客製化 message，hello 之後想要隨意的接各種訊息，這時候你可以修改你的 json 檔案，以{ }作為變數使用。
我們先將 json 檔案的內容修正為

```
{
  "message": "hello {msg} !"
}
```

同時將 App.vue 對應到 message 的部分改為

```
<h1>{{ $t("message", { msg: "World!" }) }}</h1>
```

這樣最後印出的結果自然就是 hello World!
![pre-demo2](https://i.imgur.com/e4eIVtI.png)

值得注意的是，變數的使用同樣也支援其他資料型態

**Array**

```
{
  "message": "hello {0} !"
}

<p>{{ $t('message', ['World!']) }}</p>
```

**Object**

```
{
  "message": "hello {0} !"
}

<p>{{ $t('message', {"0": World!}) }}</p>
```

**HTML format**

```
{
  "message": "hello <br> World!"
}

<p>{{ $t('message') }}</p> // 這會在hello & World中多一行空行
```

### 找不到指定翻譯檔的情況

接著我們來看一下萬一今天指定的翻譯檔中沒有找到對應的 key 值會發生什麼事情，一開始的前置作業時，你會發現在 tw.json 內並沒有 hobby 這個 key，這樣的情況就會按照你一開始在 fallback 的設定，以這個範例來說，我們預設的 fallback 語系為 en，所以它就會去 en.json 內尋找是否有 hobby 這個 key 並印出對應的值。如果連 fallback 設定的語系都找不到的話，最後就會印出 undefined 囉!

![pre-demo3](https://i.imgur.com/Rjvpgdp.png)

### 單複數的變化

翻譯檔同樣也支援單複數的變化，比方說今天我想在頁面上加入我有幾個兄弟的說明，json 檔案就可以新增以下的內容(由於英文比較有需要這個功能，我們修改 en.json 的內容即可)

```
{
  "brothers": "no brothers | one brother | {count} brothers"
}

```

在上方的範例中，訊息會根據你傳入的數字去作變化，馬上在 template 中加入吧!
首先要注意的是，當你要套用單複數的變化時，你就必須改用$tc而非原本的$t，
請在原先的內容下方加入以下的程式碼

```
<h2>I have {{ $tc("brothers", 0) }}</h2>
<h2>I have {{ $tc("brothers", 1) }}</h2>
<h2>I have {{ $tc("brothers", 2, { count: 2 }) }}</h2>
```

最終你應該會看到以下的畫面!

![pre-demo4](https://i.imgur.com/2llWiwI.png)

### 語系的切換

語系的切換也是相當的容易，在我們掛載 i18n 後，我們目前選用的語系其實存在\$i18n.locale 中，所以只要用 v-model 去控制便可以輕易的切換語系。
首先我們在 App.vue 中加上一個簡易的 select 並在 data 的部分增加目前可選的語系。

```
<select v-model="$i18n.locale">
      <option
        v-for="(lang, i) in langs"
        :key="`lang${i}`"
        :value="lang">{{lang}}
      </option>
</select>

<script>
export default {
  data() {
    return {
      langs: ["tw", "en"],
    }
  }
}
</script>
```

這下你在畫面中就可以透過簡易的 select 去切換語系囉!
![pre-demo5](https://i.imgur.com/phHnnat.png)

## 結語

## 參考資料

[vue-i18n 官方文件](http://kazupon.github.io/vue-i18n/)

[Vue i18n: Building a multi-lingual app](https://lokalise.com/blog/vue-i18n/)
