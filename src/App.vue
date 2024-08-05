<template>
  <div>
    <!-- <button @click="handleHomepage">主页</button>
    <button @click="handleAfterPlan">图书分析</button> -->
    <!-- <button @click="handlecredit">信誉分</button> -->
    <BookAnalysis v-if="showBookanalysis" :barChartData="barData" :pieChartData="pieData" />
    <ReadPlan v-if="showReadPlan" :calendarData="chartConfig" />
    <HomepageGraph v-if="showHomepage" />
    <AfterPlan v-if="showAfterPlan" :barChartData="planData" />
    <CreditGraph v-if="showCredit" :barChartData="creditData" />
    <QuestionJudge v-if="showQuestionJudge" :questionData="questionData" />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import BookAnalysis from './components/BookAnalysis.vue';
import HomepageGraph from './components/HomepageGraph.vue';
import ReadPlan from './components/ReadPlan.vue';
import AfterPlan from './components/AfterPlan.vue';
import CreditGraph from './components/CreditGraph.vue';
import QuestionJudge from "./components/QuestionJudge.vue";
const showBookanalysis = ref(false);
const showHomepage = ref(false);
const showReadPlan = ref(false);
const showAfterPlan = ref(false);
const showCredit = ref(false);
const showQuestionJudge = ref(false);
const creditData = ref({
  value: 80
});
const barData = ref({
  title: '图书指标',
  legend: ['图书名称'],
  radarIndicator: [
    { name: '想象力', max: 10 },
    { name: '语言和文笔', max: 10 },
    { name: '主题和深度', max: 10 },
    { name: '趣味性', max: 10 },
    { name: '阅读难度', max: 10 },
    { name: '影响力', max: 10 },
  ],
  seriesData: [3, 4, 5, 6, 7, 8]
});
const planData = ref({
  legend: ['计划阅读时间', '实际完成时间'],
  books: ['三体Ⅰ地球往事', '三体Ⅱ·黑暗森林', '三体Ⅲ·死神永生'],
  seriesData: [[], []]
});

const pieData = ref([
  { value: 20, name: "5分" },
  { value: 40, name: "4分" },
  { value: 15, name: "3分" },
  { value: 20, name: "2分" },
  { value: 5, name: "1分" }
]);

const questionData = ref([
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
    aiScore: 8,
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
    aiScore: 6,
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
]);
const chartConfig = ref({
  bookNames: {
    1: '图书1阅读开始',
    2: '图书2阅读开始',
    3: '图书3阅读开始'
  },
  graphData: [
    ['2024-07-13', 1],
    ['2024-07-23', 2],
    ['2024-08-01', 3],
  ],
  dateRange: ['2024-07-01', '2024-08-31'],
  visualMapPieces: [
    { min: 1, max: 1, label: '图书1', color: '#fac858' },
    { min: 2, max: 2, label: '图书2', color: '#91cc75' },
    { min: 3, max: 3, label: '图书3', color: '#5470c6' }
  ],
  readtime: [
    { start: '2024-07-13', end: '2024-07-22' },
    { start: '2024-07-23', end: '2024-07-31' },
    { start: '2024-08-01', end: '2024-08-12' }
  ]
});

const handleBookanalysis = () => {
  showBookanalysis.value = true;
  showHomepage.value = false;
  showReadPlan.value = false;
  showAfterPlan.value = false;
  showCredit.value = false;
  showQuestionJudge.value = false;
};

const handleHomepage = () => {
  showHomepage.value = true;
  showBookanalysis.value = false;
  showReadPlan.value = false;
  showAfterPlan.value = false;
  showCredit.value = false;
  showQuestionJudge.value = false;
};

const handleReadPlan = () => {

  showReadPlan.value = true;
  showBookanalysis.value = false;
  showHomepage.value = false;
  showAfterPlan.value = false;
  showCredit.value = false;
  showQuestionJudge.value = false;
};
const handleAfterPlan = () => {
  showAfterPlan.value = true;
  showBookanalysis.value = false;
  showHomepage.value = false;
  showReadPlan.value = false;
  showCredit.value = false;
  showQuestionJudge.value = false;
};
const handlecredit = () => {

  showCredit.value = true;
  showBookanalysis.value = false;
  showHomepage.value = false;
  showReadPlan.value = false;
  showAfterPlan.value = false;
  showQuestionJudge.value = false;
};
const handleQuestionJudge = () => {
  showQuestionJudge.value = true;
  showBookanalysis.value = false;
  showHomepage.value = false;
  showReadPlan.value = false;
  showAfterPlan.value = false;
  showCredit.value = false;
};
onMounted(() => {
  window.onload = function () {
    window.addEventListener('message', function (e) {
      console.log('Received message from parent:', e);
      console.log('data:', e.data);
      let jsonObject = JSON.parse(e.data);

      if (jsonObject.type.startsWith('book')) {
        let bookName = jsonObject.type.split(':')[1];

        let title = [
          bookName
        ];
        barData.value.legend = title;
        console.log('Inside bookAnalysis');
        const contentString = jsonObject.content;
        console.log("contentString:" + contentString);
        const contentObject = JSON.parse(contentString);

        const values = [
          contentObject["想象力"],
          contentObject["语言和文笔"],
          contentObject["主题和深度"],
          contentObject["趣味性"],
          contentObject["阅读难度"],
          contentObject["影响力"]
        ];
        barData.value.seriesData = values;

        handleBookanalysis();
      } else if (jsonObject.type.startsWith('homepage')) {
        handleHomepage();
      } else if (jsonObject.type === 'plan:begin') {
        console.log('Inside plan:begin');
        const contentString = jsonObject.content;
        const contentObject = JSON.parse(contentString);

        chartConfig.value.graphData = [];
        chartConfig.value.readtime = [];
        chartConfig.value.visualMapPieces = [];
        chartConfig.value.bookNames = {};

        let colors = ['#fac858', '#91cc75', '#5470c6', '#ee6666', '#73c0de', '#3ba272', '#fc8452', '#9a60b4', '#ea7ccc', '#f9d71c', '#0f4dd0'];

        let firstReadingStartDate = contentObject[0]["开始时间"];
        let lastReadingEndDate = contentObject[contentObject.length - 1]["结束时间"];

        contentObject.forEach((item, index) => {
          const bookNumber = index + 1;
          chartConfig.value.graphData.push([item['开始时间'], bookNumber]);
          chartConfig.value.readtime.push({ start: item['开始时间'], end: item['结束时间'] });
          chartConfig.value.visualMapPieces.push({
            min: bookNumber,
            max: bookNumber,
            label: '《' + item['书名'] + '》',
            color: colors[index]
          });
          chartConfig.value.bookNames[bookNumber] = `${item['书名']}阅读开始`;
        });

        chartConfig.value.dateRange = [
          `${firstReadingStartDate.substring(0, 7)}-01`,
          `${lastReadingEndDate.substring(0, 7)}-31`
        ];
        console.log('chartConfig:', chartConfig.value);

        handleReadPlan();
      } else if (jsonObject.type === 'home') {
        handleHomepage();
      } else if (jsonObject.type === 'plan:over') {
        console.log('Inside plan:after');
        const contentString = jsonObject.content;
        const contentObject = JSON.parse(contentString);
        planData.value.books = [];
        contentObject.forEach(item => {
          planData.value.books.push(item.书名);
          //planData.value.seriesData.push([]);
          console.log('item:', item.计划天数, item.实际天数);
          planData.value.seriesData[0].push(parseInt(item.计划天数));
          planData.value.seriesData[1].push(parseInt(item.实际天数));
        });
        console.log('planData:', planData.value);
        handleAfterPlan();
      } else if (jsonObject.type === 'credit') {
        const contentString = jsonObject.content;
        creditData.value = {
          value: parseInt(contentString)
        };
        console.log('creditData:', creditData);
        handlecredit();
      } else if (jsonObject.type === 'qabot') {
        console.log('Inside qabot father');
        console.log('jsonObject:', jsonObject)
        const contentString = jsonObject.content;
        console.log("final" + contentString)
        let fixedContentString = contentString.replace(/\n/g, "\\n");
        fixedContentString = fixedContentString.replace(/\r/g, "\\r");
        console.log("fixedContentString:" + fixedContentString);
        const contentObject = JSON.parse(fixedContentString);
        questionData.value = contentObject;
        console.log('questionData:', questionData.value);
        handleQuestionJudge();
      }
    });
  };
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
