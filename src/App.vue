<template>
  <div id="app">
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';

export default {
  name: 'App',
  components: {

  },
  data () {
    return {
      positiveCasesArray: [],
      hospitalizedArray: [],
      inICUArray: [],
      onVentilatorsArray: [],
      recoveredCasesArray: [],
      deathsConfirmedArray: []
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
