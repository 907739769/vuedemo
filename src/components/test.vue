<template>
    <h2 class="red">test...{{message}}</h2>
</template>

<script>
    export default {
        name: "test",
        props:{
            msg:{
                type:String,
                default:"test msg"
            }
        },
        methods:{

        },
        data(){
            return{

            }
        },
    }
</script>

<style scoped>
.red{color: red}
</style>