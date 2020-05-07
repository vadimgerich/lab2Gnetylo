
<template>
    <form v-if="visible" @submit.prevent> <!-- якщо форма видима то показати її і відмінити надсилання запиту за замовчуванням-->
        <label> ПІБ <input type="text" v-model="tournir.pib" required> </label> <br>
        <label> Країна <input type="text" v-model="tournir.country"> <inputt></label> <br>
        <label> Стать <input type="text" v-model="tournir.pol"> </label> <br>
        <label> Вік <input type="number" v-model="tournir.age"> </label> <br>
        <label> Оцінки <input type="number" v-model.number="tournir.points" min="0" step="0.01"> </label> <br>  
        <input type="button" @click="save()" value="Зберегти">     
    </form>
</template>

<script>
import setInput from "./setInput";

export default {
    name: "tournirForm",
    props:{
        //передача значення в props через value і генерування події input необхідне для коректної роботи v-model компонента
        value: Object,
        visible: Boolean
    },
    data(){
        return{
            tournir:this.value,            
        }
    },
    components:{
        setInput
    },
    methods:{
        save(){
            this.$emit('input', this.tournir);         
        }
    }
}
</script>
<style scoped>
    form{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: white ;
    }
</style>