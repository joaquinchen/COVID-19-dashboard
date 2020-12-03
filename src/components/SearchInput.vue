<template>
    <div class="search-input">
        <div class="inputarea">
            <input type="text" v-model="country" ref="input">
        </div>
        <span><img src="../../public/img/search.png" alt=""></span>
    </div>
</template>

<script>
export default {
    data(){
        return {
            country: ''
        }
    },
    methods: {
        // fireInput(){
        //     this.$refs.input.click()
        // }
    },
}
</script>

<style scoped>
.search-input{
    display: flex;
    border: 1px solid #ffffff;
    border-radius: 1.25rem;
    padding: 0.625rem 1.25rem;
    box-shadow: 0rem 0.625rem 3.75rem -1.25rem #cccccc;
    width: 355px;
    height: 3rem;
    background: #ffffff;
    position: relative;
}
.search-input input {
    border: none;
    outline: none;
    color: gray;
    width: 16rem;
    font-size: 1.4rem;
}
.inputarea{
    height: 100%;
}
span {
    position: absolute;
    right: 10px;
    top: 10px;
    width: 8%;
    cursor: pointer;
}
span img{
    width: 100%;
}
</style>