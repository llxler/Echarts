<template>
  <div class="chart-container">
    <div id="main" class="chart"></div>

  </div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref } from "vue";

function LCG(seed) {
  let _seed = seed;
  this.next = function () {
    _seed = (_seed * 1664525 + 1013904223) % 2 ** 32;
    return _seed / 2 ** 32;
  };
}
function calculateAverage(data, dim) {
  let total = 0;
  for (var i = 0; i < data.length; i++) {
    total += data[i][dim];
  }
  return (total);
}

const InitHomepage = () => {
  var chartDom = document.getElementById('main');
  var myChart = echarts.init(chartDom);
  var option;
  const seed = 12345; // 你可以选择任何你喜欢的种子值
  const rand1 = new LCG(seed);
  const rand2 = new LCG(seed);
  const rand3 = new LCG(seed);
  const rand4 = new LCG(seed);
  const data1 = [];
  const data2 = [];
  const data3 = [];
  const data4 = [];
  for (let i = 0; i < 400; i++) {
    data1.push([Math.floor(rand1.next() * 60) + 1, Math.floor(rand1.next() * 200) + 1]);
    data2.push([Math.floor(rand2.next() * 100) + 1, Math.floor(rand2.next() * 200) + 1]);
    data3.push([Math.floor(rand3.next() * 80) + 1, Math.floor(rand3.next() * 200) + 1]);
    data4.push([Math.floor(rand4.next() * 110) + 1, Math.floor(rand4.next() * 200) + 1]);
  }

  const scatterOption = (option = {
    xAxis: {
      scale: true
    },
    yAxis: {
      scale: true
    },
    series: [
      {
        type: 'scatter',
        id: 'data1',
        dataGroupId: 'data1',
        universalTransition: {
          enabled: true,
          delay: function (idx, count) {
            return Math.random() * 400;
          }
        },
        data: data1
      },
      {
        type: 'scatter',
        id: 'data2',
        dataGroupId: 'data2',
        universalTransition: {
          enabled: true,
          delay: function (idx, count) {
            return Math.random() * 400;
          }
        },
        data: data2
      },
      {
        type: 'scatter',
        id: 'data3',
        dataGroupId: 'data3',
        universalTransition: {
          enabled: true,
          delay: function (idx, count) {
            return Math.random() * 400;
          }
        },
        data: data3
      },
      {
        type: 'scatter',
        id: 'data4',
        dataGroupId: 'data4',
        universalTransition: {
          enabled: true,
          delay: function (idx, count) {
            return Math.random() * 400;
          }
        },
        data: data4
      },
    ]
  });
  const barOption = {
    xAxis: {
      type: 'category',
      data: ['东一楼', '南二楼', '北三楼', '西四楼']
    },
    yAxis: {},
    series: [
      {
        type: 'bar',
        id: 'total',
        data: [
          {
            value: calculateAverage(data1, 0),
            groupId: 'data1'
          },
          {
            value: calculateAverage(data2, 0),
            groupId: 'data2'
          },
          {
            value: calculateAverage(data3, 0),
            groupId: 'data3'
          },
          {
            value: calculateAverage(data4, 0),
            groupId: 'data4'
          },
        ],
        universalTransition: {
          enabled: true,
          seriesKey: ['data1', 'data2', 'data3', 'data4'],
          delay: function (idx, count) {
            return Math.random() * 400;
          }
        }
      }
    ]
  };
  let currentOption = scatterOption;
  setInterval(function () {
    currentOption = currentOption === scatterOption ? barOption : scatterOption;
    myChart.setOption(currentOption, true);
  }, 2000);
  option && myChart.setOption(option);
}

// setInterval(function () {
//   currentOption = currentOption === scatterOption ? barOption : scatterOption;
//   myChart.setOption(currentOption, true);
// }, 2000); 



onMounted(() => {
  InitHomepage();
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
  width: 900px;
  height: 600px;
}
</style>
