<template>
  <div id="main" class="chart-container">
    <div id="graph1" class="chart"></div>
  </div>
</template>

<script setup>
import * as echarts from 'echarts';
import { onMounted, ref, watch } from "vue";

const props = defineProps({
  calendarData: Object
});

// const chartConfig2 = {
//   bookNames: {
//     1: '三体Ⅰ地球往事',
//     2: '三体Ⅱ·黑暗森林',
//     3: '三体Ⅲ·死神永生'
//   },
//   graphData: [
//     ['2024-07-01', 1],
//     ['2024-07-11', 2],
//     ['2024-07-18', 3]
//   ],
//   dateRange: ['2024-07-01', '2024-08-31'],
//   visualMapPieces: [
//     { min: 1, max: 1, label: '三体Ⅰ地球往事', color: '#fac858' },
//     { min: 2, max: 2, label: '三体Ⅱ·黑暗森林', color: '#91cc75' },
//     { min: 3, max: 3, label: '三体Ⅲ·死神永生', color: '#5470c6' }
//   ],
//   readtime: [
//     { start: '2024-07-01', end: '2024-07-10'},
//     { start: '2024-07-11', end: '2024-07-17'},
//     { start: '2024-07-18', end: '2024-07-25' }
//   ]
// };
const PlanBeginInit = (chartConfig) => {
    console.log('Inside BookAnalysis son:');
    const chartDom = document.getElementById('graph1');
    const myChart = echarts.init(chartDom);
    const links = chartConfig.graphData.map((item, idx) => ({
        source: idx,
        target: idx + 1
    }));
    links.pop();
    console.log("fuck");
    const option = {
    tooltip: {
        formatter: function (params) {
        if (params.componentType === 'series' && params.seriesType === 'graph') {
            const value = params.data[1];
            const bookName = chartConfig.bookNames[value] || '无';
            return `${bookName}`;
        }
        return params.name || params.data[0];
        }
    },
    toolbox: {
        show: true,
        feature: {
        mark: { show: true },
        dataView: { show: true, readOnly: false },
        restore: { show: false },
        saveAsImage: { show: true },
        },
    },
    calendar: {
        top: '20%',
        left: 'center',
        orient: 'vertical',
        cellSize: [36, 36],
        yearLabel: {
        margin: 50,
        fontSize: 20
        },
        dayLabel: {
        firstDay: 1,
        nameMap: 'ZH'
        },
        monthLabel: {
        nameMap: 'ZH',
        margin: 15,
        fontSize: 14,
        color: '#999'
        },
        range: chartConfig.dateRange
    },
    visualMap: {
        min: 0,
        max: 3,
        type: 'piecewise',
        right: 5,
        top: 100,
        pieces: chartConfig.visualMapPieces,
        textStyle: {
        fontSize: 14
        },
        itemWidth: 30,
        itemHeight: 20,
        orient: 'vertical'
    },
    series: [
        {
        type: 'graph',
        edgeSymbol: ['none', 'arrow'],
        coordinateSystem: 'calendar',
        links: links,
        symbolSize: 20,
        calendarIndex: 0,
        itemStyle: {
            color: 'red',
            shadowBlur: 9,
            shadowOffsetX: 1.5,
            shadowOffsetY: 3,
            shadowColor: '#000'
        },
        lineStyle: {
            color: '#D10E00',
            width: 1,
            opacity: 1
        },
        data: chartConfig.graphData,
        z: 20
        },
        {
        type: 'heatmap',
        coordinateSystem: 'calendar',
        data: getVirtualData('2024', chartConfig),
        label: {
            show: true,
            formatter: function(params) {
            return echarts.time.format(params.value[0], '{dd}');
            },
            color: 'inherit'
            // textStyle: {
            //   color: '#FFFFFF'
            // }
        }
        }
    ]
    };
    option && myChart.setOption(option);
}

function getVirtualData(year, chartConfig) {
  const date = +echarts.time.parse(year + '-01-01');
  const end = +echarts.time.parse(+year + 1 + '-01-01');
  const dayTime = 3600 * 24 * 1000;
  const data = [];

  for (let time = date; time < end; time += dayTime) {
    const dateString = echarts.time.format(time, '{yyyy}-{MM}-{dd}', false);
    let value = 0;

    for (let i = 0; i < chartConfig.readtime.length; i++) {
      const { start, end } = chartConfig.readtime[i];
      if (dateString >= start && dateString <= end) {
        value = i + 1; // 设置值为1、2、3，对应readtime的索引加1
        break;
      }
    }

    data.push([dateString, value]);
  }

  return data;
}



onMounted(() => {

  //PlanBeginInit(chartConfig2);
  watch(() => props.calendarData, (newData) => {
  console.log('Inside BookAnalysis son watch:');
  console.log('newData', newData);
  //console.log('oldData', chartConfig2);
  if (newData) {
    PlanBeginInit(newData);
  }
}, { immediate: true });
});

// watch(() => props.calendarData, (newData) => {
//   console.log('Inside BookAnalysis son watch:');
//   console.log('newData', newData);
//   console.log('oldData', chartConfig2);
//   if (newData) {
//     PlanBeginInit(chartConfig2);
//   }
// }, { immediate: true });

</script>

<style scoped>
.chart-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20px;
}
.chart {  
  width: 35%;
  height: 700px;
  overflow: hidden;
}
</style>
