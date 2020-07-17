<template>
  <view class="swiper"
        @touchstart="onTouchStart"
        @touchmove="onTouchMove"
        @touchend="onTouchEnd"
        @touchcancel="onTouchCancel">
    <view class="swiper-wrapper" :style="{width: slidesWrapperWidth + 'px',
          transform: 'translate(' + scrollX + 'px, 0)'
        }">
      <view class="slide" :style="{width: swiperWidth}" v-for="slide in slides"
            :key="slide.id">
        {{ slide.id }}
      </view>
    </view>
  </view>
</template>

<script>
  import YrobotTouch from './YrobotTouch'
  export default {
    props: { 'slides': { type: [Array], default: function() { return [] } } },
    data() {
      console.log('data....')
      return { title: 'Hello', swiperWidth: 0, scrollX: 0 }
    },
    mounted() {
      const self = this
      uni.createSelectorQuery().in(this).select('.swiper')
        .fields({ rect: true, size: true }, function(info) {
          console.log('尺寸大小……', JSON.stringify(info), info.width)
          self.swiperWidth = info.width
        }).exec()
      console.log('onReady...')
      this.gesture = new YrobotTouch(this, 'sliderGesture', {
        tap: function() { console.log('Tap') },
        doubleTap: function() { console.log('doubleTap') },
        longTap: function() { console.log('longTap') }, //长按屏幕750ms触发
        singleTap: function() { console.log('singleTap') }, //单击屏幕触发，包括长按
        pan: function(event) {
          let scrollX = self.scrollX += event.deltaX
          if (scrollX > 0) {
            scrollX = 0
          } else if (scrollX < self.maxScrollX) {
            scrollX = self.maxScrollX
          }
          self.scrollX = scrollX
          console.log('pan: ', event.deltaX, -self.slidesWrapperWidth)
        },
        swipe: function(event) {
          console.log('swiper: ', JSON.stringify(event))
        }
      })
      console.log(this)
    },
    methods: {
      onTouchStart(event) {
        this.sliderGesture.start(event)
      },
      onTouchMove(event) {
        this.sliderGesture.move(event)
      },
      onTouchEnd(event) {
        this.sliderGesture.end(event)
      },
      onTouchCancel(event) {
        this.sliderGesture.cancel(event)
      },
      scrollToPage() {

      }
    },
    computed: {
      slidesWrapperWidth() {
        console.log('获取宽度……', this.swiperWidth * this.slides.length)
        return this.swiperWidth * this.slides.length
      },
      maxScrollX() {
        const count = this.slides.length
        return -this.slidesWrapperWidth * (count - 1) / count
      },
      slidesWrapperStyle() {
        console.log('wrapper style')
        return {
          width: this.slidesWrapperWidth + 'px',
          transform: 'translate(' + this.scrollX + 'px, 0)'
        }
      }
    }
  }
</script>

<style>
  .swiper {
    display: flex;
    flex-direction: column;
    min-height: 300rpx;
    background-color: #FF0000;
    overflow: hidden;
  }

  .swiper-wrapper {
    display: flex;
    flex: 1;
    flex-direction: row;
    background-color: #2C405A;
  }

  .slide {
    flex: 1;
    margin-right: 1px;
    background-color: #007AFF;
  }
</style>
