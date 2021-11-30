<template>
  <div class="container">
    <Spinner v-if="loading"></Spinner>
    <div id="top" v-if="!loading">
      <LineChart :labels="labels" :dataset="data" :minValue="minValue" :maxValue="maxValue"></LineChart>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import LineChart from "./LineChart.vue"
import Spinner from "./Spinner.vue"

export default {
  name: 'HelloWorld',
  components: {
    LineChart,
    Spinner
  },
  data() {
    return {
      loading: true,
      labels: [],
      data: [],
      minValue: 0,
      maxValue: 15000
    }
  },
  created() {
      this.getDataFromApi()
  },
  methods: {
      getDataFromApi() {
          axios.get('https://fo-analytics.herokuapp.com/getFollowers')
          .then(response => {
              var data = response.data.map(x => x.value);
              this.labels = response.data.map(x => x.date);
              this.data = data;
              let minValue = Math.min(...data);
              this.minValue = minValue > 500 ? minValue - 500 : minValue;
              this.maxValue = Math.max(...data) + 500;
              this.loading = false;
          })
          .catch(error => {
              this.loading = false
              console.log(error)
          })
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#top {
  width: 30%;
}

</style>
