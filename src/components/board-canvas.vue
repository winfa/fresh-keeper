<template>

  <view>
    <canvas canvas-id="boardCanvas" style="width: 750rpx; height: 550px"></canvas>
  </view>
</template>

<script>
import promisify from '@/utils/promisify.js'
import sticker from '@/components/sticker.vue'

export default {
  data () {
    return {
      scanResult: 'helo',
      message: 'my message',
      scanedCode: '',
      foodTypes: ['pork', 'beef', 'lamb', 'fish', 'seafood'],

      foodPicker: {
        list: ['pork', 'beef', 'lamb', 'fish', 'seafood'],
        currentIndex: 0
      },

      storedFoodList: [],
      showFood: {},
      status: false
    }
  },
  mounted: async function () {
    this.showFoodList()
    await this.drawBoard()
  },
  computed: {
  },

  components: {
    sticker
  },

  methods: {
    bindPickerChange (event) {
      this.foodPicker.currentIndex = event.mp.detail.value
    },

    scanCode () {
      const vm = this

      wx.scanCode({
        success (res) {
          const key = res.result
          vm.scanedCode = key
        }
      })
    },

    saveFood () {
      const vm = this
      const key = vm.scanedCode
      const foodName = vm.foodPicker.list[vm.foodPicker.currentIndex]

      const db = wx.cloud.database()
      db.collection('foods')
        .add({
          data: {
            _id: key,
            type: foodName
          }
        })
        .then(res => {
          console.log(res)
        })
    },

    showFoodList () {
      const vm = this
      const db = wx.cloud.database()

      db.collection('foods').where({}).get().then(res => {
        console.log(res)
        vm.storedFoodList = res.data
      })
    },

    getFoodListByType (type) {
      return this.storedFoodList.filter(food => food.type === type) || []
    },

    async drawBoard () {
      const ctx = wx.createCanvasContext('boardCanvas')

      ctx.setFillStyle('#f2f3f7')
      ctx.fillRect(0, 0, 450, 400)

      await this.drawSticker(ctx, '/pork.png', 0)
      // await this.drawCard(ctx, 0, 1)
      // await this.drawCard(ctx, 0, 2)
      // await this.drawCard(ctx, 0, 3)
      // await this.drawCard(ctx, 0, 4)

      await this.drawSticker(ctx, '/beef.png', 1)
      await this.drawSticker(ctx, '/goat.png', 2)
      await this.drawSticker(ctx, '/fish.png', 3)
      await this.drawSticker(ctx, '/lobster.png', 3)

      ctx.draw()
    },

    async drawSticker (ctx, src, row) {
      const lineHeight = row * 90

      ctx.beginPath()
      ctx.fillStyle = '#30c39e'
      ctx.arc(40, 78 + lineHeight, 28, 0, Math.PI * 2, true)
      ctx.fill()

      await this.drawImage(ctx, src, 20, 60 + lineHeight, 40, 40)
    },

    async drawCard (ctx, row, column) {
      const top = row * 90
      const left = column * 75

      ctx.beginPath()
      ctx.fillStyle = '#ffffff'
      ctx.rect(left, 55 + top, 60, 40)
      ctx.fill()
    },

    async drawImage (ctx, url, ...params) {
      url = `https://6672-fresh-keeper-cloud-eyrij-1259549725.tcb.qcloud.la${url}`
      const { path } = await promisify(wx.getImageInfo)({src: url})

      console.log(params)
      console.log(path)
      ctx.drawImage(path, ...params)
    }

  }
}
</script>

<style scoped>
    .action-panel {
        position: fixed;
        bottom: 50rpx;
        text-align: center;
        width: 750rpx;
        display: flex;
        justify-content: center;
    }

    .action-panel .buttons {
        width: 600rpx;
        background-color: #231c17;
        height: 120rpx;
        text-align: center;
        border-radius: 80rpx;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .action-panel .buttons img {
        width: 70rpx;
        height: 80rpx;
    }
</style>
