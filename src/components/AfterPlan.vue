<template>
    <div class="chart-container">
        <div id="bar" class="chart"></div>
    </div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref, watch } from "vue";

const props = defineProps({
    barChartData: Object,
});


const barChart = ref(null);

const AfterPlanGraph = (barChartData) => {
    console.log('Inside AfterPlan son:');
    console.log(barChartData);

    barChart.value = echarts.init(document.getElementById("bar"));

    var barOption = {
        title: {
            text: '阅读情况分析'
        },
        legend: {
            data: barChartData.legend
        },
        tooltip: {
            trigger: 'axis',
            show: true,
            confine: true,
            showContent: true,
            axisPointer: {
                type: 'cross'
            }
        },
        grid: {
            left: '7%',
            right: '7%',
            bottom: '3%',
            containLabel: true
        },
        xAxis: {
            type: 'category',
            boundaryGap: false,
            data: barChartData.books,

            axisLabel: {
                interval: 0, // 强制显示所有的标签
            },
        },
        yAxis: {
            type: 'value'
        },
        series: [
            {
                name: '计划阅读时间',
                type: 'line',

                data: barChartData.seriesData[0]
            },
            {
                name: '实际完成时间',
                type: 'line',

                data: barChartData.seriesData[1]
            }
        ]
    };
    console.log(barOption);
    barChart.value.setOption(barOption);
};

onMounted(() => {

    //PlanBeginInit(chartConfig2);
    watch(() => props.barChartData, (newData) => {
        console.log('Inside AfterPlan son watch:');
        console.log('newData', newData);
        //console.log('oldData', chartConfig2);
        if (newData) {
            AfterPlanGraph(newData);
        }
    }, { immediate: true });
});

// watch(() => props.barChartData, (newData) => {
//     if (newData) {
//         BookAnalysis(newData, props.pieChartData);
//     }
// });

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
