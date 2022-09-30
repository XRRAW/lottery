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
        <div class="wheel-base"></div>
      </div>
    </div>
    <basic-modal ref="basicModalRef" @lottery="go" />
  </div>
</template>

<script>
import BigWheel from './components/BigWheel.vue';
import BasicModal from './components/BasicModal.vue';
export default {
  components: {
    BasicModal,
    BigWheel
  },
  data() {
    return {
      allBg: 'https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/wheel_bg.png',
      prizeBgColors: ['#FEDA04', '#FFF4B4'], // 轮盘扇形颜色
      prizeList: [], // 奖品列表
      isTurning: false, // 防止连续点击
      awardsInfo: {}, // 奖品信息
    };
  },
  computed: {
    allBgStyle() {
      return `url('${this.allBg}') no-repeat top left/cover`;
    }
  },

	onLoad() {
		this.getActivityDetail();
	},

  methods: {
    // 获取活动配置详情
    getActivityDetail() {
      this.prizeList = [
				{
          prizeName: '土狗',
          prizePic: require('@/static/img/dog.jpg')
        },
        {
          prizeName: '边牧',
          prizePic: require('@/static/img/dog.jpg')
        },
        {
          prizeName: '哈士奇',
          prizePic: require('@/static/img/dog-2.jpg')
        },
        {
          prizeName: '金毛',
          prizePic: require('@/static/img/dog.jpg')
        },
        {
          prizeName: '傻狗',
          prizePic: require('@/static/img/dog.jpg')
        },
        {
          prizeName: '狗不理包子',
          prizePic: require('@/static/img/bun.png')
        }
			];
			if (this.prizeList.length > 2 && (this.prizeList.length & 1) === 1) {
				this.prizeBgColors = ['#FEDA04', '#FFF4B4', '#FFDCD7'];
			}
			this.$nextTick(() => {
        this.$refs.bigWheel.init();
      });
    },

    // 开始转动
    async go() {
			if (this.isTurning) return;
      this.isTurning = true;
			// 模拟随机数，这里可以请求后台获取中将信息
      const index = Math.floor(Math.random() * this.prizeList.length);
			// 转动转盘
			this.$refs.bigWheel.rotate(index);
    },

    // 转盘转完事件
    over(prizeInfo) {
      this.$refs.basicModalRef.openModal('4', prizeInfo);
      this.isTurning = false;
    },
	}
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
    margin-top: 114px;
    padding-bottom: 94px;
    .big-wheel-box {
      position: relative;
      text-align: center;
      font-size: 0;
      background: url('https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/disk-bg.png') no-repeat center/100%;
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
        background: url('https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/disk-base.png') no-repeat center/100%;
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