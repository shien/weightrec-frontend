<script lang="ts">
import { Component, Vue, mixins, Prop, Watch } from "nuxt-property-decorator"
import Chart from "chart.js"
import VueChart from "vue-chartjs"
const Line = VueChart.Line
const reactiveProp = VueChart.mixins.reactiveProp

@Component({})
export default class ChartLine extends mixins(Line, reactiveProp) {
  @Prop({ default: {} }) chartData!: Chart.ChartData
  @Prop({ default: {} }) options!: Chart.ChartOptions

  public mounted() {
    console.log(this)
    this.renderChart(this.chartData, this.options)
  }
}
</script>
