<template>
  <div id="linediv" ref="linediv"></div>
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
        // Create chart instance
        var chart = am4core.create(this.$refs.linediv, am4charts.XYChart);

        // Create axes
        var dateAxis = chart.xAxes.push(new am4charts.DateAxis());
        dateAxis.renderer.grid.template.location = 0.5;
        //categoryAxis.renderer.minGridDistance = 30;

        var valueAxis = chart.yAxes.push(new am4charts.ValueAxis());

        // Create series
        function createSeries(field, name, color) {
            var series = chart.series.push(new am4charts.LineSeries());
            series.dataFields.valueY = field;
            series.dataFields.dateX = "date";
            series.name = name;
            series.tooltipText = "{dateX}: [b]{valueY}[/]";
            series.strokeWidth = 2;
            series.stroke = am4core.color(color);
            series.fill = am4core.color(color);
            series.fillOpacity = 0.08
            // series.dataItems.template.locations.dateX = 0
            
            // var bullet = series.bullets.push(new am4charts.CircleBullet());
            // bullet.events.on("hit", function(ev) {
            //     alert("Clicked on " + ev.target.dataItem.dateX + ": " + ev.target.dataItem.valueY);
            // });
        }

        createSeries("cases", "Confirmed", "#3c0c52");
        createSeries("recovered", "Recovered", "#3BBD48");
        createSeries("deaths", "Deaths", "#D01414");

        chart.legend = new am4charts.Legend();
        chart.cursor = new am4charts.XYCursor();

        // let rawData = []
        // let formattedData = []
        // this.$http.get('/v3/covid-19/historical/all?lastdays=all').then(res => {
        //     rawData = res.data
        //     console.log(rawData)
        //     const dateLine = Object.keys(rawData.cases)
        //     for(let i=0; i<dateLine.length; i++){
        //         formattedData.push({
        //             date: new Date(dateLine[i]),
        //             cases: rawData.cases[dateLine[i]],
        //             recovered: rawData.recovered[dateLine[i]],
        //             deaths: rawData.deaths[dateLine[i]]
        //         })
        //     }
        //     console.log(formattedData)
        //     chart.data = formattedData
        // })

        chart.data = this.data
    },
    beforeDestroy(){
        am4core.disposeAllCharts();
    }
}
</script>

<style scoped>
#linediv {
    width: 85%;
    height: 60vh;
    /* border: 1px solid black; */
}
</style>