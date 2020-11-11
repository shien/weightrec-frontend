<template>
  <v-row>
    <v-col cols="8" sm="8" md="6">
      <v-card  height="800" width="500">
        <v-card-title class="headline">
          体重
        </v-card-title>
        <v-card-text>
        <ChartLine
          :chart-data="chartData"
          :options="chartOptions"
        />
        </v-card-text>
        <v-card-actions>
        <v-radio-group
            v-model="direction"
            hide-details
          >
            <v-radio
              value="year"
              label="Year"
            ></v-radio>
            <v-radio
              value="half-year"
              label="HalfYear"
            ></v-radio>
            <v-radio
              value="month"
              label="Month"
            ></v-radio>
            <v-radio
              value="week"
              label="Week"
            ></v-radio>
          </v-radio-group>
        </v-card-actions>
      </v-card>
      <v-card>
        <v-card-title class="headline">
          体脂肪率
        </v-card-title>
        <v-card-text>
        <ChartLine
          :chart-data="chartData"
          :options="chartOptions"
        />
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script lang="ts">
import { Component, Vue } from "nuxt-property-decorator"
import ChartLine from "@/components/VueChart.vue"
import Chart from "chart.js"

interface GridLinesSettingType {
  display: boolean,
  drawOnChartArea: boolean,
  color: string,
  zeroLineColor: string
}

const FONT_COLOR: string = "rgba(255, 255, 255, 1)"
const STACKED_SETTING: boolean = false
const GRID_LINES_SETTING: GridLinesSettingType = {
  display: true,
  drawOnChartArea: true,
  color: "rgba(255, 255, 255, .5)",
  zeroLineColor: "rgba(255, 255, 255, 1)"
}

@Component({
  components: {
    ChartLine
  }
})
export default class ChartComponent extends Vue {
  public chartTitle: string = "Vue Chartjs Demo"
  public chartData: Chart.ChartData = {}
  public chartOptions: Chart.ChartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    legend: {
      // display: false
      onClick(event, legendItem) {
        return
      },
      fullWidth: false,
      labels: {
        boxWidth: 10,
        fontColor: FONT_COLOR
      },
    },
    layout: {
      padding: {
        top: 20,
        left: 20,
        bottom: 20,
        right: 20
      }
    },
    scales: {
      xAxes: [
        {
          gridLines: GRID_LINES_SETTING,
          ticks: {
            autoSkip: true,
            fontColor: FONT_COLOR,
            fontSize: 14
          },
          scaleLabel: {
            display: true,
            fontColor: FONT_COLOR,
            labelString: "日付",
          },
          stacked: STACKED_SETTING
        },
      ],
      yAxes: [
        {
          id: "yAxis_1",
          type: "linear",
          gridLines: GRID_LINES_SETTING,
          scaleLabel: {
            display: true,
            fontColor: FONT_COLOR,
            labelString: "体重 (kg)"
          },
          ticks: {
            autoSkip: true,
            fontColor: FONT_COLOR,
            fontSize: 14,
            min: 0,
          },
          stacked: STACKED_SETTING
        },
      ],
    }
  }

  public created() {
    this.createChartData(30)
  }

  public createChartData(dayago: number) {
    var week: number[] = new Array(dayago)
    for (var i = 0; i < dayago; i++) {
        var date = new Date()
        var day = new Date(date.setDate(date.getDate() - i))
        week[i] = day.getDate()
    }
    this.chartData = {
      labels: week,
      datasets: [
        {
          yAxisID: "yAxis_1",
          type: "line",
          lineTension: 0,
          label: "体重",
          backgroundColor: "#F87979",
          borderColor: "#F87979",
          fill: false,
          data: [65, 66, 62.4, 63.3, 64.2, 64.5, 64.2]
        }
      ]
    }
  }
}
</script>
