<template>
  <view class="all">
    <view class="title">
      我们已经相处了
    </view>
    <view class="day">
      {{ days }}
    </view>
    <view class="title">天</view>
  </view>
</template>

<script>
import moment from 'moment';
export default {
  data() {
    return {
      days: 0,
    }
  },
  onLoad() {
    this.getDays();
  },
  methods: {
    getDays() {
      this.days = moment().diff(moment('2016-12-12'), 'days');
    }
  }
}
</script>

<style lang="scss" scoped>
.all {
  width: 100%;
  height: 100%;
  background: url('https://lyzh-fenhaola-manage.oss-cn-hangzhou.aliyuncs.com/images/person/bg1.png') no-repeat top left/cover;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.title {
  font-size: 28px;
  font-weight: bold;
}
.day {
  height: 72px;
  margin: 16px 0;
  padding: 0 12px;
  background: #fff;
  border-radius: 8px;
  font-size: 48px;
  font-weight: bold;
}
</style>