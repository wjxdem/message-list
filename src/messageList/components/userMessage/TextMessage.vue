<!--
 * 文本消息 
 * prop：info => {},
--> 
<template>
  <div class="text">
    <template v-for="(item,index) in textAndUrlAry">
      <!--文字消息-->
      <span v-if="item.text" :key="index" v-html="renderText(item.text)"></span>
      <!--链接消息-->
      <a
        v-if="item.url"
        :key="index"
        href="javascript:;"
        @click="handleLinkClick(item.url)"
        v-html="item.url"
      ></a>
    </template>
  </div>
</template>

<script>
// 正则寻找网址，一次只能寻找一个，需要递归[^\.,;\s\u4E00-\u9FA5]
const urlRegex = /http(s)?:\/\/((\d|[A-z]|-)*\.){1,6}([A-z]{2,6}|\d{1,3})(:\d{2,4})?(\/(\S[^\\,@;\s\u4E00-\u9FA5\u3002\uff1b\uff0c\uff1a\u201c\u201d\uff08\uff09\u3001\uff1f\u300a\u300b]*)?(((\?|#)\S*=(\S[^\\,@;\s\u4E00-\u9FA5\u3002\uff1b\uff0c\uff1a\u201c\u201d\uff08\uff09\u3001\uff1f\u300a\u300b]*)?)|\/(\S[^\\,@;\s\u4E00-\u9FA5\u3002\uff1b\uff0c\uff1a\u201c\u201d\uff08\uff09\u3001\uff1f\u300a\u300b]*)?)?)?/;
// emoji 表情正则
const emojiRegx = /(\ud83c[\udf00-\udfff])|(\ud83d[\udc00-\ude4f])|(\ud83d[\ude80-\udeff])/g;

export default {
  name: 'TextMessage',
  components: {},
  props: {
    info: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      textAndUrlAry: []
    };
  },
  watch: {
    info: {
      handler(val) {
        val ? this.renderMessage() : '';
      },
      immediate: true,
      deep:true
    }
  },
  methods: {
    /**
     * 解析消息
     */
    renderMessage() {
      const messageContent = this.info;
      if (messageContent.content) {
        const content = this.splitMessageContentTextAndUrlAry(
          typeof messageContent.content === 'string'
            ? messageContent.content
            : JSON.stringify(messageContent.content)
        );
        this.textAndUrlAry = [...content];
      } else {
        this.textAndUrlAry = [{ text: '' }];
      }
    },
    /**
     * 解析消息内容中的网址
     * @param {string} content
     */
    splitMessageContentTextAndUrlAry(content) {
      const textAndUrlAry = [];
      const splitUrlAndText = text => {
        if (!text) {
          return;
        }
        const result = urlRegex.exec(text);
        if (result) {
          textAndUrlAry.push({ text: result.input.substring(0, result.index) });
          textAndUrlAry.push({ url: result[0] });
          splitUrlAndText(
            result.input.substring(result.index + result[0].length)
          );
        } else {
          textAndUrlAry.push({ text });
        }
      };
      splitUrlAndText(content);
      return textAndUrlAry;
    },
    /**
     * 渲染文本
     * @param {string} text
     */
    renderText(text) {
      if (!text) {
        return null;
      }
      const emojiText = text
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(emojiRegx, emoji => `<b style="font-size: 20px">${emoji}</b>`);
      return emojiText;
    },
    /**
     * 链接跳转
     */
    handleLinkClick(url) {
      if (!url) {
        return;
      }
      window.open(url, '_blank');
    }
  }
};
</script>

<style lang="less" scoped>
.text {
  display: inline-block;
  position: relative;
  padding: 0 10px;
  max-width: 100%;
  line-height: 1.5;
  font-size: 15px;
  text-align: left;
  word-break: break-all;
}
</style>