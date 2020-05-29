
<!--
 * 图片消息
 * prop：info => {},
--> 
<template>
  <div>
    <img class="image" :src="`${info.content}`" @click="handleImageView" />
  </div>
</template>

<script>
export default {
  name: 'ImageMessage',
  components: {},
  props: {
    info: {
      type: Object,
      default: () => {}
    }
  },
  
  data() {
    return {};
  },
  watch: {},
  methods: {
    /**
     * 图片预览
     */
    handleImageView() {}
  }
};
</script>

<style lang="less" scoped>
.image {
  max-width: 200px;
}
</style>