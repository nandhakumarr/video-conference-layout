<template lang="pug">
.demo
  nunify-streams.streams(:streams="streams")
  .toolbar
    button(@click="addStream") Add Stream
    .separator
    select(v-model="selected")
      option(v-for="s in streams", :value="s.id") {{ s.name }}
    button(@click="removeStream(selected)", :disabled="!selected") Remove
    button(@click="removeAllStreams", :disabled="streams.length <= 0") Remove All
    .separator
    button Switch View
</template>

<script>
import f from 'faker'

import NunifyStreams from '~/components/video-conference/NunifyStreams'
export default {
  components: { NunifyStreams },
  data() {
    return {
      streams: [],
      selected: null,
    }
  },
  methods: {
    addStream() {
      this.streams.push({
        id: f.random.uuid(),
        name: f.name.findName(),
        color: f.internet.color(),
      })
    },
    removeStream(selected) {
      // this.streams = []
    },
    removeAllStreams() {
      this.streams = []
    },
  },
}
</script>

<style lang="sass" scoped>
.demo
  background: #F9f9F9
  width: 100vw
  height: 100vh
  .streams
    background: #000
    height: 90vh
    width: 90vw
    margin: 5vh 5vw
  .toolbar
    background: #24243a
    box-shadow: 0 0 10px 5px rgba(#000, 0.5)
    width: 40rem
    height: 4rem
    border-radius: 2.5rem
    display: flex
    align-items: center
    justify-content: center
    position: fixed
    margin: 0 auto
    bottom: 5rem
    left: 0
    right: 0
    z-index: 1
    button
      appearance: none
      color: white
      background-color: transparent
      padding: 0.5rem 1rem
      border-radius: 0
      border: none
      opacity: 0.8
      &:hover
        opacity: 1
      &:disabled
        opacity: 0.4
    select
      padding: 0.5rem
      width: 10rem
  .separator
    border-right: 1px solid #444
    height: 100%
    width: 1px
    margin: 0 1rem
</style>
