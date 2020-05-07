<template>
  <div id="app">    
    <messageSystem v-bind:messagesList="messagesList"></messageSystem>
    <searchString v-model="searchText"></searchString>
    <queryForm v-model="queryObject"></queryForm>
    <input type="button" @click="showNewTournirForm()" value=" Додати кнгу" />
    <tournirTable v-bind:tournirlist="filtredTournirs" @remove="removeTournir" @update="showUpdateTournirForm"></tournirTable>
    <tournirForm v-model="tournir" v-bind:visible="formVisible" @input="formInput"></tournirForm>
  </div>
</template>

<script>
import Vue from "vue";
import tournirTable from "./components/tournirTable";
import tournirForm from "./components/tournirForm";
import searchString from "./components/searchString";
import queryForm from "./components/queryForm";
import messageSystem from "./components/messageSystem";
import axios from "axios";

export default {
  //назва компоненту, співпадає із назвою файла  
  name: "app",
  //компоненти які використовуються в додатку
  components: {
    tournirTable,
    tournirForm,
    searchString,
    queryForm,
    messageSystem
  },
  //дані компонента на початку
  data() {
    return {

      tournirs: [],
      tournir: {
        title: "",
        authors: [],
        isbn: "",
        published: "1997-01-10T22:00:00.000Z",
        pages: 0,
        price: 0
      },
      //список повідомлень
      messagesList: [],
      //видимість форми
      formVisible: false,

      selectedIndex: -1,
      //текст пошуку
      searchText: "",
      //об'єкт з параметрами пошуку 
      queryObject: {
        minPages: null,
        maxPages: null,
        maxPrice: null
      }
    };
  },
  // значення які необхідно обчислити.  
  computed: {

    filtredTournir() {
      let result = this.tournirs;
      if (this.searchText)
        result = result.filter(tournir =>
          tournir.title.toLowerCase().includes(this.searchText.toLowerCase())
        );
      console.log(result);
      if (this.queryObject.minPages)
        result = result.filter(tournir => tournir.pages >= this.queryObject.minPages);
      console.log(result);
      if (this.queryObject.maxPages)
        result = result.filter(tournir => tournir.pages <= this.queryObject.maxPages);
      if (this.queryObject.maxPrice)
        result = result.filter(tournir => tournir.price <= this.queryObject.maxPrice);
      return result;
    }
  },
 
  mounted() {
    this.gettournirs().then(tournirs => {
      this.tournirs = tournirs;
    });
  },
  //методи 
  methods: {

    async getTournirs() {
      try {
        //чекаємо відповідь на запит GET  http://localhost:5000/tournir
        let resp = await axios.get("http://localhost:5000/tournir");
        //якщо все добре то повертаємо данні із відповіді на запит 
        return resp.data;
      } catch (e) {
        //якщо сталась помилка (в тому числі і сервер повернув 500)
        console.log(e);
        //додати повідомлення 
        this.messagesList.push(e);
      }
    },

    async postTournir(tournir) {
      try {
        let resp = await axios.post("http://localhost:5000/tournir", tournir);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },

    async deleteTournir(tournir) {
      try {
        let resp = await axios.delete(`http://localhost:5000/tournir/${tournir._id}`);
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },

    async patchTournir(tournir) {
      try {
        let resp = await axios.patch(
          `http://localhost:5000/tournir/${tournir._id}`,
          tournir
        );
        return resp.data;
      } catch (e) {
        console.log(e);
        this.messagesList.push(e);
      }
    },
 
    showNewTournirForm() {
      this.tournir = Object.assign(thisk.tournir, {
        pib: "",
        age: 0,
        pol: "",
        country: "",
        points: 0,
     });
      this.formAction = this.addNewTournir;
      this.formVisible = true;
    },

    showUpdateTournirForm(index) {
      this.selectedIndex = index;
      Object.assign(this.tournir, this.filtredTournirs[index]);
      this.formAction = this.updateTournir;
      this.formVisible = true;
    },
    //додавання нової книги
    addNewTournir() {
      //спочатку робимо хук 
      this.postTournir(this.tournir).

      then(tournir => this.tournirs.push(tournir));
    },

    removeTournir(index) {
      this.deleteTournir(this.filtredTournir[index]).
      then(() => {
        this.filtredTournirs.splice(index, 1);
      });
    },

    updateTournir() {
      let i = this.selectedIndex;
      this.patchTournir(this.tournir).then(tournir => {
        Object.assign(this.filtredTournirs[i], tournir);
      });
      this.selectedIndex = -1;
    },

    formInput() {
      this.formAction();
      this.formVisible = false;
    },
    formAction: function() {}
  }
};
</script>

<style scoped>
#app {
  margin: 0;
  padding: 0;
}
</style>