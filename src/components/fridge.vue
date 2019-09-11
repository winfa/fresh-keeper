<template>
  <canvas canvas-id="canvas" style="width: 750rpx; height: 550px"></canvas>
</template>

<script>
export default {
  data () {
    return {
    }
  },
  mounted: function () {
    console.log('hallo')
    this.drawBackgound()
  },

  components: {},

  methods: {
    drawBackgound () {
      const ctx = wx.createCanvasContext('canvas')
      wx.getImageInfo({
        src: 'https://6672-fresh-keeper-cloud-eyrij-1259549725.tcb.qcloud.la/cat-2177429.png',
        success: function (res) {
          console.log(res.width)
          console.log(res.path)

          ctx.drawImage(res.path, -80, -150, 500, 1000)
          ctx.draw()
        }
      })
    }
  }
}
</script>

<style scoped>
</style>

