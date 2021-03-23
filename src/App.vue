<template>
  <div id="app">
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {

  },
  async created() {
    const { data } = await axios.get('https://api.covidtracking.com/v1/us/daily.json')
    console.log(data)
  }
}
</script>
