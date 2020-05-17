<template>
  <section>
    <div class="container">
      <graph
        v-for="graph in graph_data"
        :key="graph_data.indexOf(graph) + 'graph'"
        :x="graph.x"
        :y="graph.y"
        :title="graph.title"
      />
    </div>
  </section>
</template>

<script>
import Paho from 'paho-mqtt'
import Graph from '~/components/Graph'

export default {
  components: {
    Graph
  },
  data: () => ({
    graph_data: []
  }),
  created() {
    this.client = new Paho.Client(
      'ec2-13-126-183-180.ap-south-1.compute.amazonaws.com',
      Number(8000),
      'TU00'
    )

    this.graph_data.push({
      x: [1, 2, 3, 4],
      y: [10, 15, 13, 17],
      title: 'Plot of Temperature vs time'
    })

    this.client.onConnectionLost = this.onConnectionLost
    this.client.onMessageArrived = this.onMessageArrived

    this.client.connect({ onSuccess: this.onConnect })
  },
  methods: {
    onConnect() {
      console.log('onConnect')
      this.client.subscribe('Temperature')
      this.client.subscribe('Humidity')
    },
    onConnectionLost(responseObject) {
      if (responseObject.errorCode !== 0) {
        console.log(responseObject)
      }
    },
    onMessageArrived(message) {
      console.log('onMessageArrived:' + message.payloadString)
    }
  }
}
</script>
