<template>
  <view class="bannerAll">
    <swiper
      class="swiperWrap"
      :indicator-dots="imgData.length > 1"
      :autoplay="true"
      :duration="500"
      :circular="true"
      indicator-color="rgba(256, 256, 256, 1)"
      indicator-active-color="#feda34"
    >
      <swiper-item
        class="swiperChildren"
        v-for="(item, index) in imgData"
        :key="index"
      >
        <img :src="item" />
      </swiper-item>
    </swiper>
  </view>
</template>

<script>
export default {
  props: {
    imgData: {
      type: Array,
      default: [
        "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/dog1.jpg",
        "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/dog2.jpg",
        "../../static/rxr.jpeg",
        "https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/dog3.jpg",
      ],
    },
  },
};
</script>

<style lang="scss" scope>
.bannerAll {
  width: 100%;
  img {
    width: 100%;
    height: 100%;
  }

  .swiperWrap {
    height: 270px;
  }

  ::v-deep .uni-swiper-dot {
    width: 6px;
    height: 6px;
    border-radius: 3px;
  }
}
</style>