<template>
    <div ref="mapdiv" class="chartdiv"></div>
</template>

<script>
import * as am4core from "@amcharts/amcharts4/core";
import * as am4maps from "@amcharts/amcharts4/maps";
import am4geodata_worldLow from "@amcharts/amcharts4-geodata/worldLow";
import am4geodata_data_countries2 from "@amcharts/amcharts4-geodata/data/countries2";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";

am4core.useTheme(am4themes_animated);

export default {
    data(){
        return {
            countryData: []
        }
    },
    props: ['type', 'countryList'],
    mounted(){
        var chart = am4core.create(this.$refs.mapdiv, am4maps.MapChart);

        // Set map definition
        chart.geodata = am4geodata_worldLow;

        // Set projection
        chart.projection = new am4maps.projections.Miller();

        // Create map polygon series
        var polygonSeries = chart.series.push(new am4maps.MapPolygonSeries());

        // Make map load polygon (like country names) data from GeoJSON
        polygonSeries.useGeodata = true;

        // Configure series
        var polygonTemplate = polygonSeries.mapPolygons.template;
        polygonTemplate.tooltipText = "{name}: {value}";
        polygonTemplate.fill = am4core.color("#999");

        // Create hover state and set alternative fill color
        var hs = polygonTemplate.states.create("hover");
        hs.properties.fill = am4core.color("#000099");

        // Remove Antarctica
        polygonSeries.exclude = ["AQ"];

        // Add heat rule
        polygonSeries.heatRules.push({
            "property": "fill",
            "target": polygonSeries.mapPolygons.template,
            "min": am4core.color("#ffffff"),
            "max": am4core.color("#3c0c53"),
            "logarithmic": true
        });

        // Set up heat legend
        let heatLegend = chart.createChild(am4maps.HeatLegend);
        heatLegend.series = polygonSeries;
        heatLegend.align = "left";
        heatLegend.valign = "bottom";
        heatLegend.height = am4core.percent(40);
        heatLegend.orientation = "vertical";
        heatLegend.marginRight = am4core.percent(2);
        heatLegend.marginBottom = am4core.percent(10);
        // heatLegend.valueAxis.renderer.opposite = true;
        heatLegend.valueAxis.renderer.dx = 20;
        heatLegend.valueAxis.strictMinMax = false;
        heatLegend.valueAxis.fontSize = 9;
        // heatLegend.valueAxis.maxGridDistance = 5;
        heatLegend.valueAxis.logarithmic = true;
        heatLegend.markerCount = 7;

        polygonSeries.mapPolygons.template.events.on("over", function (event) {
            handleHover(event.target);
        })

        polygonSeries.mapPolygons.template.events.on("hit", function (event) {
            handleHover(event.target);
        })

        function handleHover(column) {
            if (!isNaN(column.dataItem.value)) {
                heatLegend.valueAxis.showTooltipAt(column.dataItem.value)
            }
            else {
                heatLegend.valueAxis.hideTooltip();
            }
        }

        polygonSeries.mapPolygons.template.events.on("out", function (event) {
            heatLegend.valueAxis.hideTooltip();
        })

        // Create country specific series (but hide it for now)
        var countrySeries = chart.series.push(new am4maps.MapPolygonSeries());
        countrySeries.useGeodata = true;
        countrySeries.hide();
        countrySeries.geodataSource.events.on("done", function(ev) {
            polygonSeries.hide();
            countrySeries.show();
        });

        var countryPolygon = countrySeries.mapPolygons.template;
        countryPolygon.tooltipText = "{name}";
        countryPolygon.nonScalingStroke = true;
        countryPolygon.strokeOpacity = 0.5;
        countryPolygon.fill = am4core.color("#eee");

        var hs = countryPolygon.states.create("hover");
        hs.properties.fill = chart.colors.getIndex(9);

        // Set up click events
        polygonTemplate.events.on("hit", function(ev) {
            // console.log(ev.target)
            ev.target.series.chart.zoomToMapObject(ev.target);
            var map = ev.target.dataItem.dataContext.map;
            if (map) {
                ev.target.isHover = false;
                countrySeries.geodataSource.url = "https://www.amcharts.com/lib/4/geodata/json/" + map + ".json";
                countrySeries.geodataSource.load();
            }
        });

        // Add expectancy data
        // let data = []
        // console.log(this.countryList)
        // this.countryList.forEach(item => {
        //     data.push({
        //         id: item.countryInfo.iso2,
        //         name: item.country,
        //         value: item.cases
        //     })
        // })
        // polygonSeries.data = data
        // console.log(polygonSeries.data)
        // console.log(data)

        // let countryData = []
        // this.$http.get('/v3/covid-19/countries').then(res => {
        //     const dataList = res.data
        //     console.log(dataList)
        //     console.log(this.type)
        //     dataList.forEach(data => {
        //         this.countryData.push({
        //             id: data.countryInfo.iso2,
        //             value: data[this.type]
        //         })
        //     })
        //     console.log(this.countryData)
        //     polygonSeries.data = this.countryData
        // })

        console.log(this.countryList)
        this.countryList.forEach(data => {
            this.countryData.push({
                id: data.countryInfo.iso2,
                value: data[this.type],
                map: am4geodata_data_countries2.hasOwnProperty(data.countryInfo.iso2) ? am4geodata_data_countries2[data.countryInfo.iso2].maps[0] : null
            })
        })
        polygonSeries.data = this.countryData

        // Zoom control
        chart.zoomControl = new am4maps.ZoomControl();

        var homeButton = new am4core.Button();
        homeButton.events.on("hit", function() {
            polygonSeries.show();
            countrySeries.hide();
            chart.goHome();
        });

        homeButton.icon = new am4core.Sprite();
        homeButton.padding(7, 5, 7, 5);
        homeButton.width = 30;
        homeButton.icon.path = "M16,8 L14,8 L14,16 L10,16 L10,10 L6,10 L6,16 L2,16 L2,8 L0,8 L8,0 L16,8 Z M16,8";
        homeButton.marginBottom = 10;
        homeButton.parent = chart.zoomControl;
        homeButton.insertBefore(chart.zoomControl.plusButton);
    },
    beforeDestroy(){
        am4core.disposeAllCharts();
    }
    
}
</script>

<style scoped>
.chartdiv {
    width: 95%;
    height: 62vh;
}
</style>