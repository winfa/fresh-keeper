<template>

  <view>
    <fridge v-if="false"></fridge>
    <board></board>
  </view>

</template>

<script>
import fridge from '@/components/fridge.vue'
import board from '@/components/board.vue'

export default {
  data () {
    return {

    }
  },
  mounted: function () {
  },

  components: {
    fridge,
    board
  },

  methods: {

  }
}
</script>

<style scoped>
</style>
