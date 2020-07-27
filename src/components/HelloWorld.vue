<template>
  <article class="article-container">
    <div class="article-paragraph" v-for="paragraph in paragraphs" :key="paragraph.index">
      <div
        class="article-word"
        v-for="text in paragraph.texts"
        :key="text.index"
        @click="toggleCNState(text.word)"
      >
        <div class="article-word-cn">{{text.word.showCN?text.word.cn:''}}</div>
        <div class="article-word-en">{{text.word.en}}</div>
      </div>
    </div>
  </article>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.article-container {
  width: 80%;
  max-width: 480px;
  height: auto;
  margin: 80px auto;
  .article-paragraph {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: flex-start;
    margin-bottom: 20px;
    .article-word {
      display: block;
      margin: 0 10px 10px 0;
      cursor: default;
      .article-word-cn {
        display: block;
        height: 16px;
        width: 100%;
        font-size: 12px;
        color: #999999;
      }
      .article-word-en {
        height: 24px;
        display: block;
        font-size: 15px;
        color: #444444;
      }
    }
  }
}
</style>

<script>
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return {
      dict: {
        my: "我的",
        name: "名字",
        Ivan: "伊凡",
        I: "我, 老子, 第一人称自称",
        Chineses: "中国人",
        come: "来",
        from: "自",
        China: "中国",
        now: "现在",
        stay: "停留",
        "Kuala Lumpur": "吉隆坡",
        capital: "资本",
        city: "城市",
        Malaysia: "马来西亚",
        southeast: "东南方",
        asian: "亚洲人, 亚洲的",
        nation: "民族, 国家",
        eyes: "眼睛",
        snowing: "下雪",
        house: "房子",
        morning: "早上",
        snowman: "雪人",
      },
      article: `
My name is Ivan, I am a Chineses, and I am come from China.
Now I stay in Kuala&nbsp;Lumpur. It's the capital city of Malaysia which is a southeast asian nation.
Hooray! It's snowing! It's time to make a snowman.James runs out. He makes a big pile of snow. He puts a big snowball on top. He adds a scarf and a hat. He adds an orange for the nose. He adds coal for the eyes and buttons.In the evening, James opens the door. What does he see? The snowman is moving! James invites him in. The snowman has never been inside a house. He says hello to the cat. He plays with paper towels.A moment later, the snowman takes James's hand and goes out.They go up, up, up into the air! They are flying! What a wonderful night!The next morning, James jumps out of bed. He runs to the door.He wants to thank the snowman. But he's gone.`,
      paragraphs: [],
    };
  },
  computed: {
    // paragraphs: function () {
    //   return
    // },
  },
  mounted() {
    console.log("mounted", this.dict, this.paragraphs);
    this.paragraphs = this.article.split(/[\r\n]+/).map((texts, index) => ({
      index,
      texts: this.buildParagraph(texts),
    }));
  },
  methods: {
    buildParagraph(texts) {
      return texts.split(/\s+/).map((text, index) => ({
        index,
        word: this.buildWord(text),
      }));
    },
    buildWord(en) {
      // console.log(en)
      let cn = "",
        symbol = "";

      en = en.replace("&nbsp;", " ");
      if (en.indexOf(",") > -1) {
        symbol = ",";
        en = en.replace(",", "");
      }
      if (en.indexOf(".") > -1) {
        symbol = ".";
        en = en.replace(".", "");
      }
      if (en.indexOf("?") > -1) {
        symbol = "?";
        en = en.replace("?", "");
      }
      if (en.indexOf("!") > -1) {
        symbol = "?";
        en = en.replace("!", "");
      }

      if (this.dict[en]) {
        cn = this.dict[en];
      } else if (this.dict[en.toLowerCase()]) {
        cn = this.dict[en.toLowerCase()];
      }
      en = en + symbol;
      return { en, cn, showCN: false };
    },
    toggleCNState(word) {
      console.log(word);
      word.showCN = !word.showCN;
    },
  },
};
</script>
