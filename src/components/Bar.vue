<template>
<div class="chart-bar">
    <div class="content">
        <h2 class="content__title">Общая выручка</h2>
        <div class="content__info">
            <div class="info__item">
                <p class="info__item-title">Общее оплачено</p>
                <h2 class="info__item-data">1 123 500 <span class="valute">₽</span></h2>
            </div>
            <div class="info__item">
                <p class="info__item-title"><span class="dat dat-purple"></span> Деньги за мясо</p>
                <h2 class="info__item-data">145 200 <span class="valute">₽</span></h2>
            </div>
            <div class="info__item">
                <p class="info__item-title"><span class="dat dat-blue"></span> Расходы на ЗП</p>
                <h2 class="info__item-data">1 223 500 <span class="valute">₽</span></h2>
            </div>
            <div class="info__item">
                <p class="info__item-title"><span class="dat dat-light-blue"></span> Прочее</p>
                <h2 class="info__item-data">23 500 <span class="valute">₽</span></h2>
            </div>
        </div>
    </div>
    <div class="chart">
        <v-chart :option="option" autoresize />
    </div>
</div>
</template>
  
<script setup>
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { BarChart } from 'echarts/charts';
import { TitleComponent, TooltipComponent, LegendComponent } from 'echarts/components';
import VChart from 'vue-echarts';
import { reactive, ref } from 'vue';

use([
    CanvasRenderer,
    BarChart,
    TitleComponent,
    TooltipComponent,
    LegendComponent,
]);

const middle = reactive([]);

const data = reactive([
    {
        name: 'Мясо',
        data: [40, 120, 50, 25, 140, 160, 130, 120, 80, 90, 100, 100],
        type: 'bar',
        stack: 'x'
    },
    {
        name: 'ЗП',
        data: [60, 200, 60, 45, 50, 240, 30, 30, 100, 110, 105, 120],
        type: 'bar',
        stack: 'x'
    },
    {
        name: 'Прочее',
        data: [100, 20, 25, 60, 100, 80, 25, 20, 60, 130, 140, 140],
        type: 'bar',
        stack: 'x'
    },
    {
        name: 'Среднее',
        data: middle,
        type: 'line',
        smooth: true,
        showSymbol: false,
        legendHoverLink: false,
        symbol: 'none'
    }
]);

data[0].data.map((_, key) => {
    let sum = 0;
    data.map(type => {
        if (type.name != 'Среднее') sum += type.data[key]
    });
    middle.push(sum / data.length);
});

const option = ref({
    grid: {
        left: '30px',
        right: '30px',
        bottom: '40px',
        top: '40px',
        containLabel: true
    },
    tooltip: {
        trigger: 'axis',
        backgroundColor: '#292829',
        borderWidth: 0,
        textStyle: {
            color: '#818281'
        },
        axisPointer: {
            type: 'none'
        },
        className: 'tooltip'
    },
    itemStyle: {
        borderWidth: 3,
        borderColor: '#000',
        borderRadius: 3
    },
    color: [
        '#9747FF', '#0077F7', '#13D6FF', '#C6EC92'
    ],
    xAxis: [{
        data: ['Янв', 'Фев', 'Март', 'Апр', 'Май', 'Июнь', 'Июль', 'Авг', 'Сен', 'Окт', 'Ноя', 'Дек']
    }],
    yAxis: {
        splitLine: {
            "show": true,
            "lineStyle": {
                "color": [
                    "#292829"
                ]
            }
        },
        axisLabel: {
            fontSize: 16
        }
    },
    series: data
})
</script>

<style scoped>
.chart-bar {
  background: #1B1B1E;
  border-radius: 8px;
  padding: 40px 0 0;
}
.content {
    margin: 0 30px;
}
.content__title,
.info__item-data {
    color: #fff;
    font-size: 26px;
    text-transform: uppercase;
}
.content__info {
    margin: 30px 0 0;
    display: flex;
    justify-content: space-between;
}
.info__item {
    padding: 10px 0;
}
.info__item:not(:last-child) {
    padding: 10px 40px 10px 0;
    border-right: 2px solid #3c3c3c;
}
.info__item-title {
    text-transform: uppercase;
    font-weight: 500;
    font-size: 15px;
    color: #93969C;
}
.info__item-data {
    margin: 20px 0 0;
}
</style>