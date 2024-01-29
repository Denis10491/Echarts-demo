<template>
<div class="chart-line">
    <div class="control">
        <button 
            class="control__but but" 
            :class="{'but-active': control.all}" 
            @click="changeControl('all')"
        >Все</button>
        <button 
            class="control__but but" 
            :class="{'but-active': control.plan}" 
            @click="changeControl('plan')"
        >План</button>
        <button 
            class="control__but but" 
            :class="{'but-active': control.fact}" 
            @click="changeControl('fact')"
        >Факт</button>
    </div>

    <div class="chart">
        <v-chart :option="option" autoresize />
    </div>
</div>
</template>
  
<script setup>
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { LineChart } from 'echarts/charts';
import { TitleComponent, TooltipComponent, LegendComponent } from 'echarts/components';
import VChart from 'vue-echarts';
import { computed, reactive, ref } from 'vue';

use([
    CanvasRenderer,
    LineChart,
    TitleComponent,
    TooltipComponent,
    LegendComponent,
]);

// Data
const data = reactive([
    {
        plan: 0,
        fact: 0,
    },
    {
        plan: 30,
        fact: 20,
    },
    {
        plan: 40,
        fact: 30,
    },
    {
        plan: 60,
        fact: 60,
    },
    {
        plan: 90,
        fact: 90,
    },
    {
        plan: 115,
        fact: 115,
    },
    {
        plan: 150,
        fact: 80,
    },
    {
        plan: 180,
        fact: 100,
    },
    {
        plan: 200,
        fact: 110,
    },
    {
        plan: 240,
        fact: 140,
    },
    {
        plan: 260,
        fact: 160,
    },
    {
        plan: 280,
        fact: 160,
    },
    {
        plan: 270,
        fact: 155,
    },
    {
        plan: 290,
        fact: 120,
    },
    {
        plan: 290,
        fact: 125,
    },
    {
        plan: 290,
        fact: 120,
    },
    {
        plan: 330,
        fact: 180,
    },
    {
        plan: 350,
        fact: 195,
    },
    {
        plan: 390,
        fact: 200,
    },
    {
        plan: 400,
        fact: 210,
    },
    {
        plan: 430,
        fact: 225,
    },
    {
        plan: 445,
        fact: 215,
    },
    {
        plan: 450,
        fact: 210,
    },
    {
        plan: 460,
        fact: 235,
    },
    {
        plan: 470,
        fact: 245,
    },
    {
        plan: 480,
        fact: 260,
    },
    {
        plan: 540,
        fact: 270,
    },
    {
        plan: 550,
        fact: 280,
    },
    {
        plan: 560,
        fact: 290,
    },
    {
        plan: 575,
        fact: 300,
    },
    {
        plan: 590,
        fact: 310,
    }
]);

// Formirating yAxis
let xAxisKeys = [];
data.map((_, key) => xAxisKeys.push((key + 1 < 10) ? '0'+(key+1) : key + 1));

// Formirating Plan and Fact Data
const planData = computed(() => {
    let stash = [];
    if (control.value.all || control.value.plan) data.map(item => stash.push(item.plan));
    return stash;
});
const factData = computed(() => {
    let stash = [];
    if (control.value.all || control.value.fact) data.map(item => stash.push(item.fact));
    return stash;
});

// Control
const control = ref({
    all: true,
    plan: false,
    fact: false
});
const changeControl = (selectedControl) => {
    Object.keys(control.value).map(key => control.value[key] = false);
    control.value[selectedControl] = true;
}

// Option
const option = ref({
    grid: {
        left: '30px',
        right: '30px',
        bottom: '40px',
        top: '40px',
        containLabel: true
    },
    xAxis: [{
            data: xAxisKeys,
            boundaryGap: 0
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
    color: [
        '#0077F7', '#13D6FF'
    ],
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
    series: [
        {
            name: 'План', 
            data: planData,
            type: 'line',
            areaStyle: {},
            symbol: 'circle',
            symbolSize: 8
        },
        {
            name: 'Факт',
            data: factData,
            type: 'line',
            areaStyle: {},
            symbol: 'circle',
            symbolSize: 8
        }
    ]
});
</script>

<style scoped>
.chart-line {
  background: #1B1B1E;
  border-radius: 8px;
  padding: 40px 0 0;
}
.control {
    margin: 0 30px;
}
.control__but {
    margin: 0 8px 0 0;
}
</style>  