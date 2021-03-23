<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col text-center mt-4">
        <h1 class="display-3">COVID Visually</h1>
        <p class="lead">A no nonsense tool to quickly visualize the spread of COVID-19 in the United States</p>
      </div>
    </div>
    <div class="row mt-5" v-if="positiveCasesArray.length > 0">
      <div class="col">
        <h2>Positive</h2>
        <line-chart
          :chartData="positiveCasesArray"
          :options="chartOptions"
          label="Positive"
          >
          </line-chart>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
import LineChart from './components/LineChart'

export default {
  name: 'App',
  components: {
    LineChart
  },
  data () {
    return {
      positiveCasesArray: [],
      hospitalizedArray: [],
      inICUArray: [],
      onVentilatorsArray: [],
      recoveredCasesArray: [],
      deathsConfirmedArray: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    }


  },
  async created() {
    const { data } = await axios.get('https://api.covidtracking.com/v1/us/daily.json')
    data.forEach(d => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD")
      const { positive, hospitalizedCurrently, inIcuCurrently, onVentilatorCurrently, recovered, death} = d

      /* PUSH DESTRUCTURED DATA INTO THE RESPECTIVE ARRAYS */
      this.positiveCasesArray.push({date, total: positive})
      this.hospitalizedArray.push({date, total: hospitalizedCurrently})
      this.inICUArray.push({date, total: inIcuCurrently})
      this.onVentilatorsArray.push({date, total: onVentilatorCurrently})
      this.recoveredCasesArray.push({date, total: recovered})
      this.deathsConfirmedArray.push({date, total: death})
    });
  }
}
</script>
