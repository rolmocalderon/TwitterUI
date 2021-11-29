<template>
  <div id="top">
    <template v-if="loading">
        <div>Spinner</div>
    </template>

    <LineChart v-if="!loading" :labels="labels" :dataset="data" :minValue="minValue" :maxValue="maxValue"></LineChart>
  </div>
  
</template>

<script>
import axios from 'axios'
import LineChart from "./LineChart.vue"

export default {
  name: 'HelloWorld',
  components: {
    LineChart
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
          axios.get('http://localhost:3100/getFollowers')
          .then(response => {
              var data = response.data.map(x => x.value);
              this.labels = response.data.map(x => x.date);
              this.data = data;
              this.minValue = Math.min(...data) - 500;
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
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
