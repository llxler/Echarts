<template>
  <div class="chart-container">
    <div id="bar" class="chart"></div>
    <div id="pie" class="chart"></div>
  </div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref, watch } from "vue";

const props = defineProps({
  barChartData: Object,
  pieChartData: Array
});

const barChart = ref(null);
const pieChart = ref(null);

const BookAnalysis = (barChartData, pieChartData) => {
  console.log('Inside BookAnalysis:');
  console.log(barChartData);
  console.log(pieChartData);

  barChart.value = echarts.init(document.getElementById("bar"));
  pieChart.value = echarts.init(document.getElementById("pie"));

  var barOption = {
    title: {
      text: barChartData.title
    },
    legend: {
      data: barChartData.legend
    },
    toolbox: {
      show: true,
      feature: {
        mark: { show: true },
        dataView: { show: true, readOnly: false },
        restore: { show: true },
        saveAsImage: { show: true },
      },
    },
    radar: {
      indicator: barChartData.radarIndicator
    },
    series: [
      {
        name: 'Budget vs spending',
        type: 'radar',
        data: [
          {
            value: barChartData.seriesData,
            name: barChartData.legend[0]
          }
        ]
      }
    ]
  };

  var pieOption = {
    title: {
      text: "图书评分",
    },
    legend: {
      top: "bottom",
    },
    toolbox: {
      show: true,
      feature: {
        mark: { show: true },
        dataView: { show: true, readOnly: false },
        restore: { show: true },
        saveAsImage: { show: true },
      },
    },
    series: [
      {
        name: "Nightingale Chart",
        type: "pie",
        radius: [50, 150],
        center: ["50%", "50%"],
        roseType: "area",
        itemStyle: {
          borderRadius: 5,
        },
        data: pieChartData
      }
    ],
  };

  barChart.value.setOption(barOption);
  pieChart.value.setOption(pieOption);
};

onMounted(() => {
  BookAnalysis(props.barChartData, props.pieChartData);
});

watch(() => props.barChartData, (newData) => {
  if (newData) {
    BookAnalysis(newData, props.pieChartData);
  }
});

</script>

<style scoped>
.chart-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.chart {
  width: 600px;
  height: 400px;
}
</style>
