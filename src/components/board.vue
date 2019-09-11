<template>

  <view>

    <view v-show='scanedCode'>
      <view>选择食物</view>
      <picker @change='bindPickerChange' v-bind:value="foodPicker.currentIndex" v-bind:range='foodPicker.list'>
        <view>当前选择：{{ foodPicker.list[foodPicker.currentIndex] }}</view>
      </picker>

      <button v-on:click='saveFood()'>Save food</button>
    </view>

    <view v-show='!scanedCode'>
      <div>
        <div v-for='type in foodTypes' v-bind:key="type">
          {{ getFoodListByType(type) }}
          <sticker v-bind:type='type' v-bind:foods='getFoodListByType(type)'></sticker>
        </div>
      </div>

      <div class='action-panel'>
          <div class='buttons' @click='scanCode'>
              <img  class="img" src='https://6672-fresh-keeper-cloud-eyrij-1259549725.tcb.qcloud.la/barcode_whitle.png' />
          </div>
      </div>
    </view>

  </view>
</template>

<script>
// import promisify from '@/utils/promisify.js'
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
    // await this.drawBoard()
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
      vm.scanedCode = ''
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
