<template>
    <div>
        <div class="chart-wrap">
            <div class="title-box">line-chart</div>
            <button @click="chartDataAppend(5)">변경</button>
            <LineChart
                :chartDataX = chartDataX
                :chartDataY = chartDataY
            ></LineChart>
        </div>
        <div class="chart-wrap">
            <div class="title-box">bar-chart</div>
            <BarChart></BarChart>
        </div>
        <div class="chart-wrap">
            <div class="title-box">pie-chart</div>
            <PieChart></PieChart>
        </div>
    </div>
</template>

<script>
import LineChart from './chart/LineChart'
import BarChart from './chart/BarChart'
import PieChart from './chart/PieChart'

export default {
    name: 'VueChartJS',
    components: {
        LineChart,
        BarChart,
        PieChart,
    },

    data() {
        return {
            chartDataX : [],
            chartDataY : [],
            chrtCallData: {
                labels: [],
                data: [],
            }
        }
    },

    created () {
        this.chartDataAppend(12)
    },

    methods: {
        chartDataAppend(month) {
            let xData = [];
            let yData = [[],[]];
            for (let i = 1; i <= 31; i++) {
                let vData = i;
                xData.push( [vData, month] );
                yData[0].push( Math.floor(Math.random()*(1400-600+1)) + 600 );
                // yData[1].push( Math.floor(Math.random()*(1400-600+1)) + 600 );
            }

            this.chartDataX = xData;
            this.chartDataY = yData;
        },
    },
}
</script>

<style lang="scss" >
    @import '../assets/css/chart.scss';
</style>