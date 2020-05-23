<template>
  <section>
    <vue-plotly
      :id="uuid"
      :data="[
        {
          x: x,
          y: y,
          name: channel,
          line: { shape: 'spline', smoothing: 1.3 },
          marker: {
            color: color,
            size: 5
          }
        }
      ]"
      :layout="layout"
      :options="options"
      :auto-resize="true"
    />
  </section>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly'
import PubNub from 'pubnub'

export default {
  components: {
    VuePlotly
  },
  props: {
    title: {
      type: String,
      default: ''
    },
    uuid: {
      type: String,
      default: 'chart'
    },
    channel: {
      type: String,
      default: 'World'
    },
    color: {
      type: String,
      default: '#000'
    }
  },
  data: () => ({
    data: [],
    layout: {},
    options: {
      displaylogo: false,
      displayModeBar: false,
      responsive: true
    },
    x: [],
    y: [],
    pubnub: ''
  }),
  created() {
    this.layout = {
      title: this.title,
      yaxis: {
        fixedrange: true
      },
      xaxis: { fixedrange: true }
    }
    setInterval(() => {
      this.x.push(new Date().toLocaleTimeString())
      this.y.push(Math.random())
      // this.data = [
      //   {
      //     x: this.x,
      //     y: this.y,
      //     name: this.channel,
      //     line: { shape: 'spline', smoothing: 1.3 },
      //     marker: {
      //       color: this.color,
      //       size: 5
      //     }
      //   }
      // ]
    }, 200)

    this.pubnub = new PubNub({
      publishKey: 'pub-c-3a5226f8-981d-4669-9bdb-69d63dd8cbb2',
      subscribeKey: 'sub-c-04899be0-983f-11ea-9123-e6a08f73ae22',
      uuid: this.uuid
    })

    this.pubnub.subscribe({
      channels: [this.channel],
      withPresence: true
    })

    this.pubnub.addListener({
      message: (event) => {
        this.x.push(new Date().toLocaleTimeString())
        if (event.channel === this.channel) {
          if (typeof event.message === 'number') {
            this.y.push(event.message + Math.random())
          }
        }
      }
    })
  }
}
</script>
