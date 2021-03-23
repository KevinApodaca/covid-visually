<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col text-center mt-5">
        <h1 class="display-4">COVID Visually</h1>
        <p class="lead">A no nonsense tool to quickly visualize the COVID-19 statistics in the United States</p>
        <div class="alert alert-primary" role="alert"> Made with 💜 by <a href="https://github.com/KevinApodaca">Kevin Apodaca</a></div>
      </div>
    </div>
    <!-- POSITIVE CASES CHART -->
    <div class="row mt-5" v-if="positiveCasesArray.length > 0">
      <div class="col">
        <h2>Positive</h2>
        <line-chart :chartData="positiveCasesArray" :options="chartOptions" label="Positive" :chartColors="positiveCasesChartColors"></line-chart>
      </div>
    </div>

    <!-- HOSPITALIZED CASES CHART -->
    <div class="row mt-5" v-if="hospitalizedArray.length > 0">
      <div class="col">
        <h2>Hospitalized</h2>
        <line-chart :chartData="hospitalizedArray" :options="chartOptions" label="Hospitalized" :chartColors="hospitalizedCasesChartColors"></line-chart>
      </div>
    </div>

    <!-- ICU CASES CHART -->
    <div class="row mt-5" v-if="inICUArray.length > 0">
      <div class="col">
        <h2>In ICU</h2>
        <line-chart :chartData="inICUArray" :options="chartOptions" label="ICU" :chartColors="inICUCasesChartColors"></line-chart>
      </div>
    </div>

    <!-- VENTILATOR CASES CHART -->
    <div class="row mt-5" v-if="onVentilatorsArray.length > 0">
      <div class="col">
        <h2>On Ventilators</h2>
        <line-chart :chartData="onVentilatorsArray" :options="chartOptions" label="Ventilators" :chartColors="onVentilatorsCasesChartColors"></line-chart>
      </div>
    </div>

    <!-- RECOVERED CASES CHART -->
    <div class="row mt-5" v-if="recoveredCasesArray.length > 0">
      <div class="col">
        <h2>Recovered</h2>
        <line-chart :chartData="recoveredCasesArray" :options="chartOptions" label="Recovered" :chartColors="recoveredCasesChartColors"></line-chart>
      </div>
    </div>

    <!-- DEATHS CASES CHART -->
    <div class="row mt-5" v-if="deathsConfirmedArray.length > 0">
      <div class="col">
        <h2>Deaths</h2>
        <line-chart :chartData="deathsConfirmedArray" :options="chartOptions" label="Deaths" :chartColors="deathsConfirmedCasesChartColors"></line-chart>
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
      positiveCasesChartColors: {
        borderColor: '#077187',
        pointBorderColor: '#0E1428',
        pointBackgroundColor: '#AFD6AC',
        backgroundColor: '#21e6c1'
      },
      hospitalizedArray: [],
      hospitalizedCasesChartColors: {
        borderColor: '#251F47',
        pointBorderColor: '#260F26',
        pointBackgroundColor: '#858EAB',
        backgroundColor: '#858EAB'
      },
      inICUArray: [],
      inICUCasesChartColors: {
        borderColor: '#c5e3f6',
        pointBorderColor: '#c5e3f6',
        pointBackgroundColor: '#fc5c9c',
        backgroundColor: '#fc5c9c'
      },
      onVentilatorsArray: [],
      onVentilatorsCasesChartColors: {
        borderColor: '#ff5959',
        pointBorderColor: '#ff5959',
        pointBackgroundColor: '#facf5a',
        backgroundColor: '#facf5a'
      },
      recoveredCasesArray: [],
      recoveredCasesChartColors: {
        borderColor: '#364f6b',
        pointBorderColor: '#364f6b',
        pointBackgroundColor: '#43dde6',
        backgroundColor: '#43dde6'
      },
      deathsConfirmedArray: [],
      deathsConfirmedCasesChartColors: {
        borderColor: '#002651',
        pointBorderColor: '#002651',
        pointBackgroundColor: '#775ada',
        backgroundColor: '#775ada'
      },
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
