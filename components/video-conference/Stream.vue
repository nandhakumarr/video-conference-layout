<template lang="pug">
.stream(v-if="stream", :title="`${stream.name}: ${stream.id}`", :style="style", @click="$emit('pin', stream.id)", :class="{ pinned: stream.pin }")
  video(ref="video", autoplay, loop, @timeupdate="progress", :muted="!stream.pin")
   source(:src="video.sources[0]", type="video/mp4")
  .desc
   p {{ stream.name }}
   p {{ elapsed }}s {{ video.title }}
</template>

<script>
import videos from './videos'

export default {
  props: {
    stream: { type: Object, default: null },
    count: { type: Number, default: 0 },
    index: { type: Number, default: null },
    peerPinned: { type: Boolean, default: false },
  },
  data() {
    return { elapsed: 0 }
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
      } else if (peerPinned && this.count >= 9) {
        return 200 / this.count + '%'
      } else if (peerPinned && this.count < 9) {
        return 100 / this.count + '%'
      } else if (peerPinned) {
        return 100 / this.count + '%'
      } else {
        return null
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
      } else if (peerPinned && this.count >= 9) {
        return '10%'
      } else if (peerPinned) {
        return '20%'
      } else if (this.count > 16) {
        return 100 / 5 + '%'
      } else if (this.count >= 9) {
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
    video() {
      return videos[this.index % videos.length]
    },
  },
  watch: {
    video() {
      this.$refs.video.play()
    },
    'stream.pin': {
      deep: true,
      handler() {
        this.$refs.video.play()
      },
    },
  },
  methods: {
    progress(s) {
      const video = this.$refs.video
      this.elapsed = video ? Math.round(video.currentTime) : 0
    },
  },
  beforeDestroy() {
    const video = this.$refs.video
    if (video) {
      this.$refs.video.pause()
      this.$refs.video.removeAttribute('src')
    }
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

.stream
  position: relative
  overflow: hidden
  video
    object-fit: contain
    position: absolute
    top: 0
    right: 0
    left: 0
    bottom: 0
    margin: auto
    width: 100%
    height: 100%
    display: inline-block
    margin: auto
    vertical-align: middle
  .desc
    position: absolute
    bottom: 0.25rem
    left: 0.25rem
    background: rgba(#000, 0.4)
    color: white
    padding: 0.25rem
    p
      font-size: 0.6rem
      font-weight: 700
</style>
