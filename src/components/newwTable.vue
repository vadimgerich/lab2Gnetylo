
<template>
    <div>
        <p v-if="tournirList.length==0" class="alert">
            Турніри відсутні
        </p>
        
        <table v-if="tournirList.length>0">
            <tr>
                <th>#</th>
                <th v-on:click="sort('pib')">  ПІБ </th>
                <th v-on:click="sort('country')"> Країна  </th>
                <th v-on:click="sort('age')">  Вік </th>
                <th v-on:click="sort('pol')"> Пол </th>
                <th v-on:click="sort('points')"> Бали </th>
                <th></th>
            </tr>
            <tournirTableRow v-for="(tournir,index) in tournirList" 
                v-bind:key="tournir._id" 
                v-bind="{tournir,index}"
                @remove="remove"
                @update="update" 
            >             
            </tournirTableRow>
        </table>
    </div> 
</template>

<script>
import tournirTableRow from "./tournirTableRow";

export default {
    name:"tournirTable",
    props:{
        tournirList:Array,
    },
    data(){
        return{
           newws: this.tournirList
        }
    },
    components:{
        tournirTableRow
    },
    methods:{
        //сортування по деякому полю 
        sort(field){
           this.tournirList.sort((b1,b2)=> b1[field]>=b2[field]?1:-1);
        },
        //вилучити внигу
        remove(index){
            //генруємо подію, вилучення здійснить батьківський компонент
            this.$emit("remove",index);
        },  
        //оновити книгу
        update(index){
            //генруємо подію, оеовлення здійснить батьківський компонент
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    .alert{
        background: yellow;
        color: crimson;
    }

    table, table td{
        border-collapse: collapse;
        border: 1px solid black;
    }
</style>