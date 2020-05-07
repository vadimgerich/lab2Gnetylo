<template>
    <tr>        
        <td> {{index}} </td>
        <td> {{tournir.pib}} </td>
        <td> {{tournir.pol}} </td>
        <td> {{tournir.age}} </td>
        <td> {{tournir.country}} </td>
        <td> {{tournir.points}} </td>
        <td>
            <input type="button" value="Вилучити" @click="remove(index)">
            <input type="button" value="Редагувати" @click="update(index)">
        </td>        
    </tr>
</template>

<script>
export default {
    name:"tournirTableRow",
    props:{
        index:Number,
        tournir:Object        
    },
    data(){   
        return{

        }
    },
    methods:{        
        remove(index){
            this.$emit("remove", index);
        },
  
        update(index){
            this.$emit("update",index);
        }
    }
}
</script>

<style scoped>
    td{
        border: 1px solid black;
    }
</style>