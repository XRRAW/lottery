<template>
  <view>
    <uni-popup ref="pickerPopup" type="center" :maskClick="false">
      <view class="popup-content" :class="modalClass" :style="{ background: bgStyle }">
        <view class="closeIcon" @click="close"></view>
        <view v-if="type === '4'" class="wrap wrap4">
          <view class="awardsImg">
            <img :src="awardsInfo.prizePic" mode />
          </view>
          <view class="mainInfo">
            <span>{{ title }}</span>
            <span class="light-font" v-if="awardsInfo.prizeType === 1">
              {{ awardsInfo.rewardScore }}
            </span>
            <span>
              {{ awardsInfo.prizeType === 1 ? '积分' : awardsInfo.prizeName }}
            </span>
          </view>
          <view class="tips">
            {{ tips }}
          </view>
          <view class="modal-btn-2" @click="close">
            <img src="https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/modal-btn-2.png" />
          </view>
        </view>
      </view>
    </uni-popup>
  </view>
</template>
<script>
/**
 * ModalBasic 弹窗
 * @description 抽奖各类弹窗
 * @property {String} type = [1|2|3|4|5] 弹窗类型
 * 	@value 1 积分抽奖警告
 * 	@value 2 提交信息成功
 * 	@value 3 物品已寄出
 * 	@value 4 恭喜中奖
 * 	@value 5 填写信息
 * @event {Function} lottery 继续使用积分抽奖
 */
export default {
  name: 'modal-basic',
  props: {
    title: {
      type: String,
      default: '小轲茹中了'
    },
    tips: {
      type: String,
      default: '*请自觉吃抽中的食物'
    }
  },
  data() {
    return {
      type: '1',
      awardsInfo: {
        prizeName: '', // 奖品名称
        prizePic: '', // 奖品图片
      },
    } 
  },
  computed: {
    modalClass() {
      return this.type === '4' ? 'middleModal' : this.type === '5' ? 'bigModal' : 'smallModal';
    },
    bgStyle() {
      return `url('https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/applet/lottery/modal-bg-${this.type}.png') no-repeat top left/100% 100%`;
    }
  },
  methods: {
    openModal(type, info) {
      this.awardsInfo = info;
      this.type = type;
      this.$refs.pickerPopup.open();
    },

    close() {
      this.$refs.pickerPopup.close();
      this.$emit('popupClose');
    },
  }
};
</script>
<style lang="scss" scoped>
.popup-content {
  width: 300px;
  position: relative;
  img {
    display: block;
    width: 100%;
    height: 100%;
  }
  .closeIcon {
    width: 40px;
    height: 40px;
    position: absolute;
    top: 0;
    right: 0;
  }
}
.smallModal {
  height: 239px;
}
.middleModal {
  height: 288px;
}
.bigModal {
  height: 412px;
}
.wrap {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
}
.wrap4 {
  .awardsImg {
    width: 80px;
    height: 80px;
    margin-bottom: 21px;
  }
  .mainInfo {
    font-size: 14px;
    font-weight: bold;
    color: #aa5833;
    line-height: 22px;
    margin-bottom: 12px;
    .light-font {
      font-size: 24px;
      color: #F34837;
    }
  }
  .tips {
    font-size: 12px;
    color: #b2826d;
    line-height: 17px;
    margin-bottom: 3px;
  }
  .modal-btn-2 {
    width: 203px;
    height: 65px;
    margin-bottom: 13px;
  }
}
</style>
