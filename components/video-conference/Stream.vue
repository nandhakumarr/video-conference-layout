<template lang="pug">
.stream(v-if="stream", :title="`${stream.name}: ${stream.id}`", :style="style", @click="$emit('pin', stream.id)", :class="{ pinned: stream.pin }")
</template>

<script>
export default {
  props: {
    stream: { type: Object, default: null },
    count: { type: Number, default: 0 },
    peerPinned: { type: Boolean, default: false },
  },
  computed: {
    style() {
      const backgroundColor = this.stream ? this.stream.color : 'transparent'
      const { minWidth, maxWidth, minHeight, maxHeight, height, flex } = this
      return {
        minWidth,
        maxWidth,
        minHeight,
        maxHeight,
        height,
        flex,
        backgroundColor,
      }
    },
    minHeight() {
      const peerPinned = this.peerPinned && !this.stream.pin
      if (this.stream.pin) {
        return '100%'
      } else if (peerPinned && this.count > 8) {
        return 100 / 8 + '%'
      } else if (peerPinned) {
        return 100 / this.count + '%'
      } else {
        return 'auto'
      }
    },
    maxHeight() {
      return this.minHeight
    },
    height() {
      if (this.stream.pin) {
        return '100%'
      } else {
        return 'auto'
      }
    },
    minWidth() {
      const peerPinned = this.peerPinned && !this.stream.pin
      if (this.stream.pin) {
        return '80%'
      } else if (peerPinned && this.count > 9) {
        return '10%'
      } else if (peerPinned) {
        return '20%'
      } else if (this.count > 16) {
        return 100 / 5 + '%'
      } else if (this.count > 9) {
        return 100 / 4 + '%'
      } else if (this.count > 6) {
        return 100 / 3 + '%'
      } else if (this.count > 3) {
        return 100 / 2 + '%'
      } else {
        return 'auto'
      }
    },
    maxWidth() {
      return this.minWidth
    },
    flex() {
      const peerPinned = this.peerPinned && !this.stream.pin
      if (peerPinned) {
        return 'auto'
      } else {
        return '1 1 auto'
      }
    },
    alignSelf() {
      const peerPinned = this.peerPinned && !this.stream.pin
      if (this.stream.pin) {
        return 'flex-start'
      } else if (peerPinned) {
        return 'flex-end'
      } else {
        return null
      }
    },
  },
}
</script>

<style lang="sass" scoped>
.stream
  color: white
  min-height: 2rem
  min-width: 2rem
  width: auto
  height: auto
  &.pinned
    width: 80%
    height: 100%
    position: absolute
    left: 0
    top: 0
</style>
