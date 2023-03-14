<template>
  <div class="all" :style="{ background: allBgStyle }">
    <div class="wheel-wrap">
      <div class="big-wheel-box">
        <big-wheel
          ref="bigWheel"
          width="240px"
          height="240px"
          borderColor="rgba(0,0,0,0)"
          :prizeList="prizeList"
          :prizeBgColors="prizeBgColors"
          :turnsTime="2.5"
          @over="over"
        >
          <template v-slot:item="{ item }">
            <div class="prizeWrap">
              <div class="prize-name">{{ item.prizeName }}</div>
              <img class="prize-img" :src="item.prizePic" />
            </div>
          </template>
        </big-wheel>

        <!-- 开始按钮 -->
        <img
          class="btn-go"
          @click="go"
          src="https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/go-pointer.png"
        />
        <div class="wheel-base">
          <span>小轲茹有</span>
          <span class="light-font">{{ number }}</span>
          <span>次抽奖机会</span>
        </div>
      </div>
    </div>
    <basic-modal ref="basicModalRef" tips="*请在活动结束前找小随领取" @lottery="go" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      allBg:
        "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/bg2.png",
      prizeBgColors: ["#FEDA04", "#FFF4B4"], // 轮盘扇形颜色
      prizeList: [], // 奖品列表
      isTurning: false, // 防止连续点击
      awardsInfo: {}, // 奖品信息
      number: 0,
    };
  },
  computed: {
    allBgStyle() {
      return `url('${this.allBg}') no-repeat top left/cover`;
    },
  },

  onLoad() {
    this.getActivityDetail();
    this.number = uni.getStorageSync('times');
  },

  onShareAppMessage() {
    return {
      title: "任轲茹的基地",
      imageUrl: "../../static/rxr.jpeg",
    };
  },

  methods: {
    // 获取活动配置详情
    getActivityDetail() {
      this.prizeList = [
        {
          prizeName: "玫瑰",
          prizePic:
            "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/rose.jpg",
        },
        {
          prizeName: "蛋糕",
          prizePic:
            "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/cake.jpg",
        },
        {
          prizeName: "口红",
          prizePic:
            "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/lipstick.jpg",
        },
        {
          prizeName: "神仙水",
          prizePic:
            "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/skII.jpg",
        },
      ];
      if (this.prizeList.length > 2 && (this.prizeList.length & 1) === 1) {
        this.prizeBgColors = ["#FEDA04", "#FFF4B4", "#FFDCD7"];
      }
      this.$nextTick(() => {
        this.$refs.bigWheel.init();
      });
    },

    // 开始转动
    async go() {
      if (this.isTurning) return;
      if (!uni.getStorageSync('times')) return;
      this.number = 0;
      uni.setStorageSync('times', 0);
      this.isTurning = true;
      // 模拟随机数，这里可以请求后台获取中奖信息
      const index = Math.floor(Math.random() * this.prizeList.length);
      // 转动转盘
      this.$refs.bigWheel.rotate(index);
    },

    // 转盘转完事件
    over(prizeInfo) {
      this.$refs.basicModalRef.openModal("4", prizeInfo);
      this.isTurning = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.all {
  width: 100vw;
  min-height: 100vh;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  .wheel-wrap {
    width: auto;
    margin-top: 173px;
    padding-bottom: 94px;
    .big-wheel-box {
      position: relative;
      text-align: center;
      font-size: 0;
      background: url("https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/disk-bg.png")
        no-repeat center/100%;
      padding: 35px;
      .prizeWrap {
        display: flex;
        flex-direction: column;
        align-items: center;
      }
      .prize-name {
        width: 72px;
        margin-top: 9px;
        word-break: break-all;
        font-size: 12px;
        font-weight: bold;
        line-height: 14px;
        text-align: center;
        color: #aa5833;
      }
      .prize-img {
        width: 50px;
        height: 50px;
      }
      .btn-go {
        position: absolute;
        top: 100px;
        left: 50%;
        transform: translateX(-50%);
        width: 90px;
        height: 106.2px;
      }
      .wheel-base {
        width: 311px;
        height: 100px;
        position: absolute;
        top: 304px;
        left: 50%;
        transform: translateX(-50%);
        background: url("https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/disk-base.png")
          no-repeat center/100%;
        font-size: 14px;
        font-weight: bold;
        text-align: center;
        color: #ffd4d3;
        line-height: 147px;
        .light-font {
          font-size: 16px;
          color: #ffcb28;
        }
      }
    }
  }
}
</style>