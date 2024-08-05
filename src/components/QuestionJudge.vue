<template>
    <div id="main" class="chart-container">
        <div id="pie" class="chart"></div>
        <div id="bar" class="chart"></div>

    </div>
    <div class="table-container">
        <el-table :data="tableData" style="width: 100%" :row-class-name="tableRowClassName">
            <el-table-column prop="index" label="题目序号" width="100" align="center"></el-table-column>
            <el-table-column prop="question" label="题目" align="center"></el-table-column>
            <el-table-column prop="correctAnswer" label="参考答案" align="center"></el-table-column>
            <el-table-column prop="userAnswer" label="我的答案" align="center"></el-table-column>
            <el-table-column prop="score" label="AI 评分" align="center"></el-table-column>
            <el-table-column prop="reason" label="AI 评语" align="center"></el-table-column>
        </el-table>
    </div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref, watch } from "vue";

const props = defineProps({
    questionData: Object,
});

const barChart = ref(null);
const pieChart = ref(null);

const questionbarData = ref({
    //data:题目序号
    data: [],
    //seriesdata:得分
    seriesData: []
});
const questionpieData = ref([
    //value:得分，扣分
    { value: 0, name: "得分" },
    { value: 0, name: "失分" },
]);
const tableData = ref([
    {
        index: 1,
        question: '题目 1',
        referenceAnswer: '参考答案 1',
        myAnswer: '我的答案 1',
        aiScore: 7,
        aiComment: '评语 1',
    },
    {
        index: 2,
        question: '题目 2',
        referenceAnswer: '参考答案 2',
        myAnswer: '我的答案 2',
        aiScore: 3,
        aiComment: '评语 2评语 2评语 2评语 2评语 2评语 2评语 2评语 2',
    },
    {
        index: 3,
        question: '题目 3',
        referenceAnswer: '参考答案 3',
        myAnswer: '我的答案 3',
        aiScore: 7,
        aiComment: '评语 3',
    },
    {
        index: 4,
        question: '题目 4',
        referenceAnswer: '参考答案 4',
        myAnswer: '我的答案 4',
        aiScore: 3,
        aiComment: '评语 4评语 4评语 4评语 4评语 4评语 4评语 4评语 4',
    },
    {
        index: 5,
        question: '题目 5',
        referenceAnswer: '参考答案 5',
        myAnswer: '我的答案 5',
        aiScore: 7,
        aiComment: '评语 5',
    },
    {
        index: 6,
        question: '题目 6',
        referenceAnswer: '参考答案 6',
        myAnswer: '我的答案 6',
        aiScore: 3,
        aiComment: '评语 6评语 6评语 6评语 6评语 6评语 6评语 6评语 6',
    },
    // 更多数据...
])

const tableRowClassName = ({ row }) => {
    if (row.score > 5) {
        return 'success-row'
    } else {
        return 'warning-row'
    }
}

const QuestionGraph = (barChartData, pieChartData) => {
    console.log('Inside QuestionJudge son:');
    console.log(barChartData);
    pieChart.value = echarts.init(document.getElementById("pie"));
    barChart.value = echarts.init(document.getElementById("bar"));

    var barOption = {
        title: {
            text: "得分曲线",
        },
        tooltip: {
            trigger: 'item'
        },
        xAxis: {
            type: 'category',
            boundaryGap: false,
            data: barChartData.value.data,
            axisLabel: {

            }
        },
        yAxis: {
            type: 'value',
            max: 10 // 固定纵轴最大值为10
        },
        series: [
            {
                data: barChartData.value.seriesData,
                type: 'line',
                areaStyle: {}
            }
        ]
    };
    var pieOption = {
        title: {
            text: '你的总分'
        },
        tooltip: {
            trigger: 'item'
        },
        legend: {
            top: '3%',
            left: 'center',
            itemWidth: 30,  // 调整图例项的宽度
            itemHeight: 20,  // 调整图例项的高度
            textStyle: {
                fontSize: 16  // 调整图例项文字的大小
            }
        },
        series: [
            {
                name: "分数",
                type: "pie",
                radius: ['40%', '70%'],
                avoidLabelOverlap: false,
                padAngle: 5,
                itemStyle: {
                    borderRadius: 10,
                    color: function (params) {
                        // 根据数据项的索引设置颜色
                        return params.dataIndex === 0 ? '#67c23a' : '#f1190b';
                    }
                },
                label: {
                    show: false,
                    position: 'center'
                },
                emphasis: {
                    label: {
                        show: true,
                        fontSize: 40,
                        fontWeight: 'bold'
                    }
                },
                labelLine: {
                    show: false
                },
                data: pieChartData.value
            }
        ],
    };

    barChart.value.setOption(barOption);
    pieChart.value.setOption(pieOption);
};

onMounted(() => {

    //PlanBeginInit(chartConfig2);

    watch(() => props.questionData, (newData) => {
        console.log('Inside AfterPlan son watch:');
        console.log('newData', newData);
        //console.log('oldData', chartConfig2);
        if (newData) {
            tableData.value = newData
            console.log('newData', newData);
            console.log('tableData', tableData);
            questionbarData.value.data = tableData.value.map(item => `第${item.index}题`);
            questionbarData.value.seriesData = tableData.value.map(item => item.score)
            tableData.value.forEach(item => {

                questionpieData.value[0].value += parseInt(item.score)
                console.log(item.score)
                questionpieData.value[1].value += 10 - parseInt(item.score)

            })

            console.log(questionbarData.value)
            console.log(questionpieData.value)
            QuestionGraph(questionbarData, questionpieData);
            //AfterPlanGraph(tableData);
        }
    }, { immediate: true });
});

</script>

<style>
.chart-container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
}

.chart {
    width: 40%;
    height: 400px;
    overflow: hidden;
}

.table-container {
    margin: 0 auto;
    width: 80%;
}

.el-table .warning-row {
    --el-table-tr-bg-color: var(--el-color-warning-light-9);
}

.el-table .success-row {
    --el-table-tr-bg-color: var(--el-color-success-light-9);
}

.el-table th,
.el-table td {
    text-align: center;
    color: #000;
}

.el-table th {
    font-weight: bold;
    color: #000;
    border-top: 1px solid #d3d3d3;
    line-height: 2.5;
    /* 增加表头的高度 */
}
</style>
