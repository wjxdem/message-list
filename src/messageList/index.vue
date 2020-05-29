
<!--
 * @Author: jesse wu
 * @Description: 消息列表
--> 
<template>
  <div class="message-container" style="background-color: rgb(239, 239, 239);">
    <div class="view">
      <div class="title">
        <p>demo</p>
      </div>
      <!-- data is empty -->
      <div class="loading">
        <div class="empty" v-if="message && message.length==0">
          <span>暂无消息记录</span>
        </div>
      </div>

      <template v-for="(item,index) in message">
        <SystemMessage
          v-if="item.messageType===MessageType.SystemMessage"
          :key="index"
          :info="item.info"
        />
        <div v-else :key="index" :class="['message',item.isMy?'my':'other']">
          <div class="icon">
            <img
              class="avatar"
              width="45"
              height="45"
              :src="item.isMy? 'http://img.52z.com/upload/news/image/20180111/20180111085521_86389.jpg': 'https://img.mp.itc.cn/q_70,c_zoom,w_640/upload/20160819/f3558b8f68ca47e0b6a2d66c31ad38fe_th.jpg'"
            />
          </div>
          <div class="block">
            <div class="name">{{item.userName}}</div>
            <div v-if="item.messageType===MessageType.TextMessage" class="content">
              <TextMessage :info="item.info" />
            </div>
            <div
              v-if="item.messageType===MessageType.ImageMessage"
              :style="{'text-align':item.isMy?'right':'left'}"
            >
              <ImageMessage :info="item.info" />
            </div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import TextMessage from "./components/userMessage/TextMessage";
import ImageMessage from "./components/userMessage/ImageMessage";
import SystemMessage from "./components/systemMessage/index";

export default {
  name: "MessageList",
  components: {
    TextMessage,
    ImageMessage,
    SystemMessage
  },
  props: {
    message: {
      type: Array,
      default: () => [
        {
          messageType: "TextMessage",
          isMy: true,
          userName: "Jesse",
          info: {
            content: "hello world!😃,132431419934,https://www.baidu.com"
          }
        },
        {
          messageType: "TextMessage",
          userName: "zack",
          isMy: false,
          info: {
            content: "你好！😁"
          }
        },
        {
          messageType: "SystemMessage",
          info: {
            infoType: "recall",
            nameList: []
          }
        },
        {
          messageType: "ImageMessage",
          userName: "zack",
          isMy: false,
          info: {
            content:
              "https://icweiliimg1.pstatp.com/weili/bl/312261353904799797.jpg"
          }
        },
        {
          messageType: "TextMessage",
          isMy: true,
          userName: "Jesse",
          info: {
            content: "你的头像真好看！"
          }
        },
        {
          messageType: "TextMessage",
          userName: "zack",
          isMy: false,
          info: {
            content: "是吗，随便选的"
          }
        },
        {
          messageType: "SystemMessage",
          info: {
            infoType: "add",
            nameList: ["name1", "name2"]
          }
        },
        {
          messageType: "SystemMessage",
          info: {
            infoType: "recall",
            nameList: []
          }
        }
      ],
      required: false
    }
  },
  data() {
    return {
      MessageType: {
        TextMessage: "TextMessage", // 文本消息
        ImageMessage: "ImageMessage", // 文本消息
        SystemMessage: "SystemMessage" // 系统消息
      }
    };
  },
  watch: {},
  methods: {}
};
</script>

<style lang="less" scoped>
.message-container {
  width: 100%;
  height: 100%;
  z-index: 100;
  position: fixed;
  left: 0;
  top: 0;
  overflow: hidden;
  .view {
    box-shadow: 1px 1px 20px -5px #000;
    min-width: 300px;
    background: #f5f5f5;
    margin: 0 auto;
    overflow: auto;
    padding: 0;
    position: relative;
    z-index: 101;
    max-height: 800px;
    width: 400px;
    .title {
      text-align: center;
      color: #ccc;
      width: 100%;
      height: 50px;
      line-height: 50px;
      font-size: 14px;
      p {
        padding: 0;
        margin: 0;
      }
    }
  }
  .empty {
    margin-top: 100px;
    text-align: center;
    font-size: 14px;
  }
  /*一条消息*/
  .message {
    padding: 10px 15px 10px 20px;
    /* 头像 */
    .icon {
      width: 45px;
      height: 45px;
      overflow: hidden;
      border-radius: 3px;
      background: #888;
      user-select: none;
    }
    /* 消息块 */
    .block {
      width: calc(100% - 120px);
      .name {
        font-size: 12px;
        color: #999;
        margin-top: -2px;
        user-select: none;
      }
      .content {
        max-width: 600px;
        margin-top: 5px;
        background: #fff;
        display: inline-block;
        border-radius: 4px;
        padding: 8px 9px;
        font-size: 14px;
      }
    }
    /* 对方的消息 */
    &.other {
      overflow: hidden;
      .icon {
        float: left;
      }
      .block {
        float: left;
        margin-left: 10px;
        position: relative;

        .name {
          text-align: left;
        }
        .content {
          float: left;
          background: #fff;
          &:before {
            content: "";
            position: absolute;
            left: -10px;
            top: 25px;
            width: 0;
            height: 0;
            background: #000;
            border: 5px solid #fff;
            border-top-color: #f3f3f3;
            border-left-color: #f3f3f3;
            border-bottom-color: #f3f3f3;
          }
        }
      }
      .lk_checkbox_warpper {
        float: left;
        margin-right: 15px;
        margin-top: 7px;
      }
    }
    /* 我的消息 */
    &.my {
      overflow: hidden;
      .icon {
        float: right;
      }
      .block {
        float: right;
        margin-right: 10px;
        position: relative;

        .name {
          text-align: right;
        }
        .content {
          float: right;
          background: #9eea6a;
          &:before {
            content: "";
            position: absolute;
            right: -10px;
            top: 25px;
            width: 0;
            height: 0;
            background: #000;
            border: 5px solid #9eea6a;
            border-top-color: #f3f3f3;
            border-right-color: #f3f3f3;
            border-bottom-color: #f3f3f3;
          }
        }
      }
      .lk_checkbox_warpper {
        float: left;
        margin-top: 7px;
      }
    }
    &.time {
      user-select: none;
      text-align: center;
      font-size: 12px;
      color: #999;
    }
  }
}
</style>