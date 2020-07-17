<template>
  <view class="container">
    <euv-swiper class="swiper"
                :swiperStyle="swiperStyle"
                :slides="slides">
    </euv-swiper>
  </view>
</template>

<script>
  import swiper from '../../components/euv-swiper/euv-swiper.vue'
  export default {
    components: { 'euv-swiper': swiper },

    data() {
      console.log('data....')
      return {
        title: 'Hello',
        slides: [{ id: 1 }, { id: 2 }, { id: 3 }],
        swiperStyle: {
          display: 'flex',
          flexDirection: 'column',
          height: '200px',
          backgroundColor: '#FF00FF',
          overflow: 'hidden'
        }
      }
    },
    onReady() {
      this.$emit('haha')
    },
    methods: {

    },
  }
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    flex: 1;
    background-color: #FF0000;
  }

  .swiper {
    display: flex;
    flex-direction: column;
    min-height: 400rpx;
    background-color: #FF00FF;
    overflow: hidden;
  }
</style>
