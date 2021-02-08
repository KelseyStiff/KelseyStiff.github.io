<!-- no template -->
<script>
import { Doughnut, mixins } from 'vue-chartjs'
let { reactiveProp } = mixins

export default {
  extends: Doughnut,
  name: 'MacrosCharts',
  mixins: [ reactiveProp ],
  data() {
    return {
      chartOptions: {
        legend: { display: false },
        scales: {
          xAxes: [
            {
              type: 'donut'
            }
          ]
        }
      }
    }},
  mounted() {
    this.renderChart(this.chartData, this.chartOptions)
  }
}
</script>