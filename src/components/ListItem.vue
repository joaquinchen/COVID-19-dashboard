<template>
    <div class="list-item">
        <img :src="data.countryInfo.flag">
        <div class="info">
            <h3 style="font-size: 1rem">{{data.country}}</h3>
            <ul>
                <li><span style="color: #1d254e;">{{`${data.cases}`}}</span></li>
                <li><span style="color: #3BBD48;">{{`${data.recovered}`}}</span></li>
                <li><span style="color: #D01414;">{{`${data.deaths}`}}</span></li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        data: {type: Object}
    },
    
}
</script>

<style scoped>
.list-item{
    display: flex;
    align-items: center;
    /* border: 1px solid #000; */
    height: 40px;
    margin-top: 8px;
}
.list-item:first-child{
    margin-top: 0;
}
img{
    width: 20%;
    height: 100%;
    display: inline-block;
}
.info{
    width: 72%;
    height: 40px;
    margin-left: 8px;
}
.info h3{
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
}
.info ul{
    list-style-type: none;
}
.info li{
    width: 33%;
    display: inline-block;
} 
</style>