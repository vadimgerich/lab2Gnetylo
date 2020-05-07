//форма для запитів
<template>
    <form @submit.prevent>
        Вік не менший за<input type="number" v-model.number="query.age"> 
        <input type="button" value="Шукати" @click="search()">
        <input type="button" value="Очистити" @click="reset()">
    </form>
</template>
<script>
export default {
    props:{
        value:Object
    },
    data(){
        return{
            query:this.value
        }
    },
    methods:{
        reset(){
            this.query.age = null;
            this.$emit("input",this.query);
        },
        search(){            
            this.$emit("input",this.query);
        }
    }
}
</script>