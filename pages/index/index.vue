<template>
  <view class="container" @click="showVueGestureDemo">
    <load-swiper :slides="slides"></load-swiper>
  </view>
</template>

<script>
  import swiper from '../../components/lod-swiper/lod-swiper.vue'
  export default {
    components: { 'load-swiper': swiper},
    data() { return { slides: [{ id: 0 }, { id: 1 }, { id: 2 }], title: 'Hello' } },
    onLoad() {},
    methods: {
      showVueGestureDemo() {
        uni.navigateTo({
          url: '../euvGesture/euvGesture'
        })
      },
      onSwipe(event) {
        console.log('swipe...', JSON.stringify(event))
      },
      onPan(event) {
        console.log('pan: ', JSON.stringify(event))
      }
    }
  }
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    flex: 1;
    min-height: 100vh;
    width: 100%;
    background-color: #4CD964;
  }
</style>
