<template>
  <view class="gesture-view"
        :style="{height: swiperHeight + 'px'}"
        @touchstart="handleTouchStart"
        @touchmove="handleTouchMove"
        @touchend="handleTouchEnd"
        @touchcancel="handleTouchCancel">
        <slot></slot>
  </view>
</template>

<script>
  import gestureMixin from './mixins/euv-gesture/euv-gesture'
  export default {
    mixins: [gestureMixin],
    props: {swiperHeight: {type: Number}}
  }
</script>

<style>
  .gesture-view {
    display: flex;
    flex: 1;
  }
</style>
