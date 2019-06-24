<template>
  <div>
    hello
    <button @click="scanCode">xxx</button>
    {{ scanResult }}
  </div>
</template>

<script>

export default {
  data () {
    return {
      scanResult: 'helo'
    }
  },

  components: {
  },

  methods: {
    scanCode () {
      var that = this

      wx.scanCode({
        success (res) {
          that.scanResult = res.result
          console.log(res.result)
          console.log('.........')
        }
      })


    }
  }

}
</script>

<style scoped>

</style>
