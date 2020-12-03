<template>
    <div class="placeholder">
        <div class="navbar">
            <h1>Covid-19 Tracker</h1>
            <div class="date"><p>{{`${this.date.getFullYear()} ${this.date.getMonth()+1} ${this.date.getDate()} ${this.date.getHours()}:${this.date.getMinutes()}:${this.date.getSeconds()}`}}</p></div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            date: new Date()
        }
    },
    methods: {
        setDate(){
            setInterval(() => {
                this.date = new Date()
            }, 1000)
        }
    },
    mounted(){
        // this.setDate()
        setInterval(() => {
            this.date = new Date()
        }, 1000)
    },
    // computed: {
    //     getTime(){
    //         return `${this.date.getFullYear()} ${this.date.getMonth()+1} ${this.date.getDate()} ${this.date.getHours()}:${this.date.getMinutes()}:${this.date.getSeconds()}`
    //     }
    // }
}
</script>

<style scoped>
.navbar{
    background: #3C0C53 linear-gradient(hsla(0, 0%, 100%, 0.1), transparent);
    display: flex;
    justify-content: space-between;
    padding: 0.5rem 1.25rem;
    color: #ffffff;
    line-height: 2.5rem;
}
.navbar .date{
    font-size: 1.6rem;
}
</style>