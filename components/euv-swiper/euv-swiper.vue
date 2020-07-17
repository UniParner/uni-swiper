<template>
  <euv-gesture-view class="swiper1"
                    :swiperHeight="swiperHeight"
                    @onTap="onTap($event)"
                    @onSwipe="onSwipe($event)"
                    @onSwipeLeft="onSwipeLeft($event)"
                    @onSwipeRight="onSwipeRight($event)"
                    @onPanStart="onPanStart"
                    @onPanMove="onPanMove"
                    @onPanEnd="onPanEnd"
                    @onPan="onPan($event)">
    <view class="swiper-wrapper"
          :animation="animation"
          :style="{width: slidesWrapperWidth + 'px', height: swiperHeight + 'px', transform: 'translate(' + scrollX + 'px, 0)'}">
      <view class="slide"
            :style="{width: swiperWidth, backgroundColor:slideBgs[index] }"
            v-for="(slide, index) in slides"
            :key="slide.id">
        {{ slide.id }}
      </view>
    </view>
  </euv-gesture-view>
</template>

<script>
  import gestureView from './euv-gesture-view.vue'
  import { DIRECTION_LEFT } from './mixins/euv-gesture/config'
  export default {
    components: { 'euv-gesture-view': gestureView },
    props: {
      'slides': {
        type: [Array],
        default: function() { return [] }
      },
      swiperStyle: { type: Object, default () { return {} } }
    },
    data() {
      return {
        swiperWidth: 0,
        swiperHeight: 0,
        scrollX: 0,
        page: 0,
        animation: {},
        slideBgs: [
          '#FFFF00', '#00FFFF', '#FF00FF'
        ]
      }
    },
    computed: {
      slidesWrapperWidth() {
        console.log('euv-swiper: 获取宽度……', this.swiperWidth * this.slides.length)
        return this.swiperWidth * this.slides.length
      },
      maxScrollX() {
        const count = this.slides.length
        return -this.slidesWrapperWidth * (count - 1) / count
      },
      slidesWrapperStyle() {
        // console.log('euv-swiper: wrapper style')
        return {
          width: this.slidesWrapperWidth + 'px',
          transform: 'translate(' + this.scrollX + 'px, 0)'
        }
      }
    },
    methods: {
      onTap(event) {
        console.log(`onTap`)
      },
      onPanStart(e) {
        this.offsetX = this.scrollX
        // console.log(`${this.scrollX} onPanStart: ${JSON.stringify(e)} `)
      },
      onPanMove(e) {
        // console.log(`onPanMove: ${JSON.stringify(e)}`)
      },
      onPanEnd(e) {
        const isPanleft = e.direction == DIRECTION_LEFT
        const x = parseInt(Math.abs(this.scrollX))
        let page = parseInt(x / this.swiperWidth)
        const extraPercent = (x % this.swiperWidth) / this.swiperWidth
        if (isPanleft) {
          page = extraPercent > 0.2 ? ++page : page
        } else {
          page = extraPercent > 0.8 ? ++page : page
        }
        if (page < 0) { page = 0 }
        if (page >= this.slides.length) { page = this.slides.length - 1 }
        this.page = page
        this.animateToPage()
      },
      onPan(e) {
        if (isNaN(this.offsetX)) {
          console.log('不是数字……')
          return
        }
        let panX = this.offsetX + e.moveStatus.x
        if (panX > 0) {
          panX = 0
        } else if (panX < this.maxScrollX) {
          panX = this.maxScrollX
        }
        this.anim.translateX(panX).step({ duration: 0 })
        this.animation = this.anim.export()
        this.scrollX = panX
        console.log(`偏移值为${this.scrollX} ${panX} ${this.offsetX}`)
      },
      onSwipe(e) {
        // console.log(`onSwipe: ${JSON.stringify(e)}`)
      },
      onSwipeLeft(e) {
        // if (this.page == 0) { return }
        // this.page = this.page - 1
        // console.log(`onSwipeLeft: ${JSON.stringify(e)}`)
      },
      onSwipeRight(e) {
        // if (this.page == this.slides.length - 1) { return }
        // this.page = this.page + 1
        // console.log(`onSwipeRight: ${JSON.stringify(e)}`)
      },
      animateToPage() {
        console.log('处理动画……')
        const offset = this.page * this.swiperWidth
        this.anim.translateX(-offset).step({ duration: 250 })
        this.animation = this.anim.export()
        setTimeout(() => {
          this.scrollX = -offset
          this.offsetX = undefined
        }, 260)
      }
    },
    watch: {
      page(newPage, oldPage) {
        this.$emit("onPageChange", newPage)
      }
    },
    created() {
      this.anim = uni.createAnimation()
    },
    mounted() {
      const self = this
      uni.createSelectorQuery().in(this).select('.swiper1')
        .fields({ rect: true, size: true }, function(info) {
          // console.log('尺寸大小……', JSON.stringify(info), info.width)
          self.swiperWidth = parseInt(info.width)
          self.swiperHeight = info.height
        }).exec()
    }
  }
</script>

<style>
  .swiper1 {
  }

  .swiper-wrapper {
    display: flex;
    flex-direction: row;
    background-color: #2C405A;
  }

  .slide {
    flex: 1;
    background-color: #007AFF;
  }
</style>
