<template>
  <div class="sticker">
    <view class="sticker-header">
      <img class="sticker-img" v-bind:src="imageBaseURL + stickerInfo[type].src" />
    </view>
    <view class="sticker-counter">{{ foods.length }}</view>
  </div>
</template>

<script>
export default {
  props: ['type', 'foods'],
  data() {
    return {
      imageBaseURL:
        'https://6672-fresh-keeper-cloud-eyrij-1259549725.tcb.qcloud.la',
      stickerInfo: {
        pork: {
          src: '/pork.png',
        },
        beef: {
          src: '/beef.png',
        },
        lamb: {
          src: '/goat.png',
        },
        fish: {
          src: '/fish.png',
        },
        seafood: {
          src: '/lobster.png',
        },
      },
    }
  },

  methods: {
    getStickerImageSrc(type) {
      const baseURL =
        'https://6672-fresh-keeper-cloud-eyrij-1259549725.tcb.qcloud.la'

      if (type === 'pork') {
        return `${baseURL}/pork.png`
      }
    },
  },
}
</script>

<style scoped>
.sticker {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 30px 0 0 30px;
}
.sticker-header {
  background-color: #30c39e;
  border-radius: 50%;
  width: 56px;
  height: 56px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.sticker-img {
  width: 40px;
  height: 40px;
}
.sticker-counter {
  padding: 0 30px 0 0;
}
</style>
