<template>
    <div class="card">
        <h2 class="title">{{title}}</h2>
        <p class="count" ref="count">
            <!-- {{count}} -->
            <ICountUp
                :delay="4000"
                :endVal="count"
                :options="options"
            />
        </p>
        <span>{{todayCount}}</span>
    </div>
</template>

<script>
import ICountUp from 'vue-countup-v2';

export default {
    data(){
        return {
            show: false,
            options: {
                useEasing: true,
                useGrouping: true,
                separator: ',',
                decimal: '.',
                prefix: '',
                suffix: ''
            }
        }
    },
    components: {
        ICountUp
    },
    props: {
        title: {
            type: String
        },
        count: {
            type: Number
        },
        todayCount: {
            type: Number
        },
        color: {
            type: String
        }
    },
    mounted(){
        // console.log(this.count)
        this.$refs.count.style.color = this.color
    }
}
</script>

<style scoped>
.card {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid #ffffff;
    background: #ffffff;
    border-radius: 1.25rem;
    padding: 0.625rem 1.25rem;
    box-shadow: 0rem 0.625rem 3.75rem -1.25rem #cccccc;
    /* width: 16rem; */
    width: 22%;
    height: 130px;
    margin: 10px;
    cursor: pointer;
}
.title{
    font-weight: normal;
    font-family: sans-serif;
}
.count{
    font-size: 2.6rem;
}
</style>