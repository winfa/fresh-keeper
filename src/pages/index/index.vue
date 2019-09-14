<template>
  <view>
    <fridge v-if="false"></fridge>
    <board></board>
  </view>
</template>

<script>
import board from '@/components/board.vue'

export default {
  data() {
    return {}
  },
  mounted: function() {},

  components: {
    board,
  },

  methods: {},
}
</script>

<style scoped>
</style>
