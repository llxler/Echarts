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

const CreditInit = (score) => {
    console.log('Inside Credit son:');
    console.log(score);
    barChart.value = echarts.init(document.getElementById("bar"));

    var barOption = {
        title: {
            text: '您的信誉分',
            left: 'center',
            top: '5',
            textStyle: {
                fontSize: 18,
                color: '#333'
            }
        },
        series: [
            {
                type: 'gauge',
                progress: {
                    show: true,
                    width: 12
                },
                axisLine: {
                    lineStyle: {
                        width: 10
                    }
                },
                axisTick: {
                    show: false
                },
                splitLine: {
                    length: 10,
                    lineStyle: {
                        width: 2,
                        color: '#999'
                    }
                },
                axisLabel: {
                    distance: 15,
                    color: '#999',
                    fontSize: 15
                },
                anchor: {
                    show: true,
                    showAbove: true,
                    size: 25,
                    itemStyle: {
                        borderWidth: 10
                    }
                },
                title: {
                    show: true
                },
                detail: {
                    valueAnimation: true,
                    fontSize: 40,
                    offsetCenter: [0, '70%']
                },
                data: [
                    {
                        value: score
                    }
                ]
            }
        ]
    };


    barChart.value.setOption(barOption);
};

onMounted(() => {
    watch(() => props.barChartData, (newData) => {
        if (newData) {
            CreditInit(newData.value);
        }
    }, { immediate: true });
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
    width: 420px;
    height: 280px;
}
</style>
