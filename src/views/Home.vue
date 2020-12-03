<template>
  <div class="home">
    <div class="dashboard">
      <Card v-if="result.cases" @click="type='cases'" title='Comfirmed' :count="result.cases" :todayCount="result.todayCases" color="#1d254e"/>
      <Card v-if="result.cases" @click="type='active'" title='Active' :count="result.active" color="#C38F29"/>
      <Card v-if="result.cases" @click="type='recovered'" title='Recovered' :count="result.recovered" :todayCount="result.todayRecovered" color="#3BBD48"/>
      <Card v-if="result.cases" @click="type='deaths'" title='Death' :count="result.deaths" :todayCount="result.todayDeaths" color="#D01414"/>
    </div>
    <div class="sidebar">
      <List :dataList="dataList"/>
    </div>
    <div class="searchbar">
      <SearchInput />
    </div>
    <div class="display">
      <Map v-if="getReady && showMap" :type="type" :countryList="countryList"/>
      <LineChart v-if="getReady && showLine" :data="formattedData"/>
      <ColumnChart v-if="getReady && showColumn" :data="result"/>
      <div class="display-control">
        <button @click="showMap=true; showLine=false; showColumn=false">Map</button>
        <button @click="showMap=false; showLine=true; showColumn=false">Line</button>
        <button @click="showMap=false; showLine=false; showColumn=true">column</button>
      </div>
    </div>
  </div>
</template>

<script>
import Card from '@/components/Card'
import List from "@/components/List"
import SearchInput from '@/components/SearchInput'
import Map from '@/components/Map'
import LineChart from '@/components/Line'
import ColumnChart from '@/components/Column'

export default {
  name: 'Home',
  components: {
    Card,
    List,
    SearchInput,
    Map,
    LineChart,
    ColumnChart
  },
  data(){
    return {
      result: {},
      dataList: [],
      type: 'cases',
      countryList: [],
      showMap: true,
      showColumn: false,
      showLine: false,
      getReady: false,
      formattedData: []
    }
  },
  methods: {
    sortByProperty(property){
      return function(obj1, obj2){
        let value1 = obj1[property]
        let value2 = obj2[property]
        return value2 - value1
      }
    },
    getData(){
      this.$http.get('/v3/covid-19/all').then(res => {
        // console.log(res)
        this.result = res.data
        // this.showColumn = true
        return this.$http.get('/v3/covid-19/countries')
      }).then(res => {
        this.dataList = res.data.sort(this.sortByProperty('cases'))
        this.countryList = res.data
        // this.showMap = true
        return this.$http.get('/v3/covid-19/historical/all?lastdays=all')
      }).then(res => {
        let rawData = res.data
        this.formattedData = []
        console.log(rawData)
        const dateLine = Object.keys(rawData.cases)
        for(let i=0; i<dateLine.length; i++){
            this.formattedData.push({
                date: new Date(dateLine[i]),
                cases: rawData.cases[dateLine[i]],
                recovered: rawData.recovered[dateLine[i]],
                deaths: rawData.deaths[dateLine[i]]
            })
        }
        // this.showLine = true
        this.getReady = true
        console.log(this.formattedData)
      })
    }
  },
  created(){
    this.getData()
    setInterval(() => {
      this.getData()
    }, 1000*60);
    // this.$http.get('/v3/covid-19/countries').then(res => {
    //   // console.log(res.data.slice(100,110))
    //   this.dataList = res.data
    //   this.countryList = res.data
    //   console.log(123, this.countryList)
    //   // console.log(this.dataList)
    // })
  }
}
</script>

<style scoped>
.dashboard{
  display: flex;
  flex-wrap: wrap;
  width: 75vw;
  margin: 20px;
  margin-left: 10px;
  /* border: 1px solid #000; */
}
.sidebar{
  position: absolute;
  /* top: 77px;
  right: 20px; */
  top: 160px;
  right: 25px;
}
.searchbar{
  position: absolute;
  top: 87px;
  right: 30px;
}
.display{
  width: 68vw;
  height: 64.5vh;
  /* border: 1px solid #000; */
  margin-left: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}
.display-control{
  position: absolute;
  right: 10px;
  top: 10px;
  /* border: 1px solid #000; */
  display: flex;
  flex-direction: column;
  align-items: center;
}
.display-control button{
  border: 1px solid transparent;
  background-color: #3c184d;
  color: #ffffff;
  cursor: pointer;
  outline: none;
  margin-top: 4px;
  width: 60px;
}
</style>