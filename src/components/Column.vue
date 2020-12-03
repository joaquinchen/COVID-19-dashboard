<template>
    <div id="columndiv" ref="columndiv"></div>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

export default {
    data(){
        return {

        }
    },
    props: ['data'],
    mounted(){
        var chart = am4core.create(this.$refs.columndiv, am4charts.XYChart);

        // chart.marginRight = 400;

        // Add data
        // chart.data = [{
        // "country": "Lithuania",
        // "research": 501.9,
        // "marketing": 250,
        // "sales": 199
        // }, {
        // "country": "Czech Republic",
        // "research": 301.9,
        // "marketing": 222,
        // "sales": 251
        // }, {
        // "country": "Ireland",
        // "research": 201.1,
        // "marketing": 170,
        // "sales": 199
        // }, {
        // "country": "Germany",
        // "research": 165.8,
        // "marketing": 122,
        // "sales": 90
        // }, {
        // "country": "Australia",
        // "research": 139.9,
        // "marketing": 99,
        // "sales": 252
        // }, {
        // "country": "Austria",
        // "research": 128.3,
        // "marketing": 85,
        // "sales": 84
        // }, {
        // "country": "UK",
        // "research": 99,
        // "marketing": 93,
        // "sales": 142
        // }, {
        // "country": "Belgium",
        // "research": 60,
        // "marketing": 50,
        // "sales": 55
        // }, {
        // "country": "The Netherlands",
        // "research": 50,
        // "marketing": 42,
        // "sales": 25
        // }];

        console.log(this.data)
        let formattedData = [
            {type: 'Confirmed', number: this.data.cases},
            {type: 'Recovered', number: this.data.recovered},
            {type: 'Deaths', number: this.data.deaths}
        ]
        chart.data = formattedData
        
        //console.log('chart', chart);

        // Create axes
        var categoryAxis = chart.xAxes.push(new am4charts.CategoryAxis());
        categoryAxis.dataFields.category = "type";
        categoryAxis.title.text = "Total Cases";
        categoryAxis.renderer.grid.template.location = 0;
        categoryAxis.renderer.minGridDistance = 20;

        var valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
        valueAxis.title.text = "Cases (M)";

        // Create series
        var series = chart.series.push(new am4charts.ColumnSeries());
        series.dataFields.valueY = "number";
        series.dataFields.categoryX = "type";
        // series.name = "Research";
        series.tooltipText = "{type}: [bold]{number}[/]";
        series.stacked = true;
        series.fill = am4core.color('#e00000')
        series.stroke = am4core.color('#e00000')
        

        // Add cursor
        chart.cursor = new am4charts.XYCursor();
    },
    beforeDestroy(){
        am4core.disposeAllCharts();
    }
}
</script>

<style scoped>
#columndiv {
    width: 88%;
    height: 60vh;
    /* border: 1px solid black; */
}
</style>