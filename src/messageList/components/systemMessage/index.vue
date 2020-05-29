<!--
 * 系统消息
 * prop：info => {},
--> 
<template>
  <div class="system">
    <div class="content" v-html="tpl"></div>
  </div>
</template>

<script>

export default {
  name: 'SystemMessage',
  components: {},
  props: {
    info: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      infoType: {
        add: 'add', // 拉人
        kicked: 'kicked', // 踢人
        recall: 'recall' // 撤回消息
      },
      tpl: ''
    };
  },
  watch: {
    info: {
      handler(val) {
        val ? this.renderMessage(val) : '';
      },
      immediate: true,
      deep: true
    }
  },
  methods: {
    /**
     * 解析消息
     */
    renderMessage() {
      if (!Array.isArray(this.info.nameList)) {
        return;
      }
      switch (this.info.infoType) {
        case this.infoType.add:
          this.tpl = `你邀请 ${this.info.nameList.join()} 加入群组`;
          break;
        case this.infoType.kicked:
          this.tpl = `你将${this.info.nameList.join()} 移除群组`;
          break;
        default:
          this.tpl = `你撤回了一条消息`;
          break;
      }
    }
  }
};
</script>

<style lang="less" scoped>
.system {
  margin: 10px 0;
  text-align: center;
  .content {
    display: inline-block;
    padding: 2px 5px;
    font-size: 12px;
    color: #fff;
    border-radius: 4px;
    background-color: #dadada;
  }
}
</style>