<template>
  <div class="container">
    <div>
      <h1 class="title">
        VerneMQ - MQTT broker + NodeMCU
      </h1>
      <h2 class="subtitle">
        Plot real-time chart of DHT sensor connected to NodeMCU module.
      </h2>
      <div class="links">
        <nuxt-link class="button is-info" to="dashboard">
          Start
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {}
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
