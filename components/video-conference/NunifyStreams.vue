<template lang="pug">
.nunify-streams(:class="{ pinned: hasPinned }")
  .fill-space(v-if="hasPinned")
  stream(v-for="s in streams", :key="s.id", :stream="s", :count="streams.length", @pin="pin", :peerPinned="hasPinned")
</template>

<script>
import Vue from 'vue'
import Stream from './Stream'
export default {
  components: {
    Stream,
  },
  props: {
    streams: { type: Array, default: () => [] },
  },
  computed: {
    hasPinned() {
      let result = false
      this.streams.forEach((stream) => {
        if (stream.pin) {
          result = true
        }
      })
      return result
    },
  },
  methods: {
    pin(id) {
      console.log(id)
      this.streams.forEach((stream) => {
        Vue.set(stream, 'pin', stream.id === id)
      })
    },
  },
}
</script>

<style lang="sass" scoped>
.nunify-streams
  display: flex
  align-items: stretch
  justify-content: center
  flex-wrap: wrap
  position: relative
  box-sizing: border-box
  &.pinned
    flex-direction: column
  .fill-space
    width: 80%
    height: 100%
</style>
