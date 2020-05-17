<template>
  <section>
    <Plotly
      :data="[
        {
          x: x,
          y: y
        }
      ]"
      :layout="{
        title: title
      }"
      :display-mode-bar="false"
    ></Plotly>
  </section>
</template>

<script>
import { Plotly } from 'vue-plotly'

export default {
  components: {
    Plotly
  },
  props: {
    x: {
      type: Array,
      default: () => []
    },
    y: {
      type: Array,
      default: () => []
    },
    title: {
      type: String,
      default: ''
    }
  }
}
</script>
