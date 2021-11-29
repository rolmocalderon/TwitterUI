<script>
import { Line } from 'vue3-chart-v2'

export default {
  extends: Line,
  props: {
      labels: Array,
      dataset: Array,
      minValue: Number,
      maxValue: Number
  },
  mounted() {
    this.renderChart(
      {
        labels: this.labels,
        datasets: [
          {
            label: "Followers",
            data: this.dataset,
            backgroundColor: "transparent",
            borderColor: "rgba(1, 116, 188, 0.50)",
            pointBackgroundColor: "rgba(171, 71, 188, 1)"
          }
        ]
      },
      {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
            yAxes: [{
                display: true,
                ticks: {
                    suggestedMin: this.minValue,
                    suggestedMax: this.maxValue
                }
            }]
        },
      }
    );
  }
};
</script>