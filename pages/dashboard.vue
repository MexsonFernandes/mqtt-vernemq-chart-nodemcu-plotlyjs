<template>
  <section>
    <div class="container">
      <graph
        v-for="graph in graph_data"
        :key="graph.uuid"
        :uuid="graph.uuid"
        :channel="graph.channel"
        :title="graph.title"
        :color="graph.color"
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
    this.graph_data.push({
      title: 'Temperature vs time graph',
      uuid: 'chart1',
      channel: 'Temperature',
      color: '#add8e6'
    })

    // this.graph_data.push({
    //   title: 'Humidity vs time graph',
    //   uuid: 'chart2',
    //   channel: 'Humidity',
    //   color: '#eeb26a'
    // })
    // this.client = new Paho.Client(
    //   'ec2-13-126-183-180.ap-south-1.compute.amazonaws.com',
    //   Number(8000),
    //   'TU00' + new Date().getMilliseconds().toString()
    // )
    // this.client.onConnectionLost = this.onConnectionLost
    // this.client.onMessageArrived = this.onMessageArrived
    // this.client.connect({ onSuccess: this.onConnect })
  },
  methods: {
    onConnect() {
      console.log('onConnect')
      this.client.subscribe('Temperature')
      this.client.subscribe('Humidity')

      this.client.subscribe('World')
      const message = new Paho.Message('Hello')
      message.destinationName = 'World'
      this.client.send(message)
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
