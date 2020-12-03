<template>
    <div class="list">
        <div class="list-container">
            <ListItem v-for="data in dataList" :data="data" :key="data.countryInfo._id" />
        </div>
    </div>
</template>

<script>
import ListItem from '@/components/ListItem'

export default {
    data(){
        return {

        }
    },
    components: {
        ListItem
    },
    props: {
        dataList: {type: Array}
    }
}
</script>

<style scoped>
.list {
    width: 360px;
    height: 270px;
    box-shadow: 0rem 0.625rem 3.75rem -1.25rem #cccccc;
    background: #ffffff;
    /* margin: 10px; */
    border-radius: 1.25rem;
    display: flex;
    justify-content: center;
    align-items: center;
}
.list-container{
    width: 320px;
    height: 230px;
    overflow-y: auto;
}
</style>