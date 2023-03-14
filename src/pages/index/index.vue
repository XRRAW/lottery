<template>
  <view class="all">
    <banner />
    <view class="mainWrap">
      <view
        class="imageWrap"
        v-for="(item, index) in funList"
        :key="index"
        :class="item.class"
        @click="jumpTo(item.url)"
      >
        <view class="urlName">{{ item.name }}</view>
      </view>
    </view>
  </view>
</template>

<script>
import { funList } from "./z.constants";
export default {
  data() {
    return {
      funList,
    };
  },

  methods: {
    jumpTo(url) {
      uni.navigateTo({
        url,
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.all {
  height: 100%;
}
.mainWrap {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.imageWrap {
  width: 340px;
  height: 64px;
  margin-top: 16px;
  display: flex;
  align-items: center;
  &.eat {
    background: url("https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/fun1.png")
      no-repeat left top/100%;
  }
  &.holiday {
    background: url("https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/fun2.png")
      no-repeat left top/100%;
  }
}
.urlName {
  margin-left: 100px;
  font-size: 18px;
  font-weight: bold;
  color: rgba(0, 0, 0, 0.85);
}
</style>