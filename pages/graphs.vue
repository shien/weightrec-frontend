<template>
  <v-row>
    <v-col cols="8" sm="8" md="6" align-self="center">
     <v-card>
        <v-toolbar>
          <v-toolbar-title>期間</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-select
              :items="items"
              label="選択"
              return-object
              single-line
            ></v-select>
          </v-toolbar-items>
        </v-toolbar>
      </v-card>
      <v-card>
        <v-card-title class="headline">
          {{ weightChartTitle }}
        </v-card-title>
        <ChartLine
          :chart-data="weightChartData"
          :options="weightChartOptions"
        />
      </v-card>
      <v-card>
        <v-card-title class="headline">
          {{ bodyFatPercentageChartTitle }}
        </v-card-title>
        <v-card-text>
        <ChartLine
          :chart-data="bodyFatPercentageChartData"
          :options="bodyFatPercentageChartOptions"
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
  },
})
export default class ChartComponent extends Vue {
  public items = ['一年', '半年', '1ヶ月', '1週間']
  public weightChartTitle: string = "体重"
  public weightChartData: Chart.ChartData = {}
  public weightChartOptions: Chart.ChartOptions = {
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
    const days = 180
    this.createWeightChartData(days)
    this.createBodyFatPercentageChartData(days)
  }

  public createWeightChartData(dayago: number) {
    var week: string[] = new Array(dayago)
    for (var i = 0; i < dayago; i++) {
        var date = new Date()
        var day = new Date(date.setDate(date.getDate() - i))
        var monthDay = day.getMonth() + "/" + day.getDate()
        week[dayago - i - 1] = monthDay
    }
    this.weightChartData = {
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

  public bodyFatPercentageChartTitle: string = "体重"
  public bodyFatPercentageChartData: Chart.ChartData = {}
  public bodyFatPercentageChartOptions: Chart.ChartOptions = {
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
            labelString: "体脂肪率 (%)"
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
  public createBodyFatPercentageChartData(dayago: number) {
    var week: string[] = new Array(dayago)
    for (var i = 0; i < dayago; i++) {
        var date = new Date()
        var day = new Date(date.setDate(date.getDate() - i))
        var monthDay = day.getMonth() + "/" + day.getDate()
        week[dayago - i - 1] = monthDay
    }
    this.bodyFatPercentageChartData = {
      labels: week,
      datasets: [
        {
          yAxisID: "yAxis_1",
          type: "line",
          lineTension: 0,
          label: "体脂肪率",
          backgroundColor: "#F87979",
          borderColor: "#F87979",
          fill: false,
          data: [31.1, 28.2, 28.5, 25.2, 25.4, 22.2, 22.9]
        }
      ]
    }
  }
}
</script>