<script>
import Products from "@/components/Products.vue";
import axiosInstance from "../services/axios.js";


export default {
  name: 'App',
  components: {Products},
  data(){
    return{

      userObject:{
        name: 'John',
        age: 20,
      },
      inputName:'',
      inputAge: null,
      price : 5,
      products: [],
      checkbox: false,
      textRule: [],
      categories: [],
      text: 'Категорії продуктів: ',
      showJewelery: false,
      jewelery: [],

    }
  },
  methods:{
    getJewelery(){
      axiosInstance.get('products/category/jewelery')
          .then(response => {
       this.jewelery = response.data;
          })
          .catch(error=>{
            console.log(error)
          })
    },
    getCategory(){
      axiosInstance.get('/products/categories')
          .then(response => {
            this.categories = response.data;
          })
          .catch(error=>{
            console.log(error)
          })
    },
    getProducts(){
      axiosInstance.get('/products')
          .then(response => {
            this.products = response.data;
          })
          .catch(error=>{
            console.log(error)
          })
    },
    inputObject(event){
      this.textRule = [];
      event.target.name === 'name' ? this.inputName = event.target.value : this.inputAge = event.target.value
    },
    changeObject(){
      this.textRule = [];
      if(this.inputAge && this.inputName){
        this.userObject = { name: this.inputName, age: this.inputAge };
      } else {
        !this.inputAge ? this.textRule.push('Введіть вік') : null
        !this.inputName ? this.textRule.push('Введіть ім\'я') : null
      }
    }

  },
  computed: {
    computed(){
  return "Об'єкт змінить дані на: name - " + this.inputName +  " age - " + this.inputAge
  },
    combinedData(){
      return this.text + this.categories
    }
  },
  mounted() {
    this.getProducts(),
    this.getCategory()
  },
  watch:{
    price(newVal, oldVal){
      console.log("Змінилось значення price на - " + newVal + " попереднє значення - " + oldVal)
    },
    "userObject.name"(newVal, oldVal){
      console.log("Змінилось значення userObject.name на - " + newVal + " попереднє значення - " + oldVal)
},
    checkbox: {
      handler(newVal, oldVal) {
        console.log(newVal,oldVal)
      },
      immediate: true
    },
    "userObject": {
      handler(newVal, oldVal) {
        console.log(newVal,oldVal)
      },
      deep: true
    },
    showJewelery(newVal, oldVal) {
    this.getJewelery()
    },
  computed(newVal,oldVal){
      console.log('Зміни в computed property ( computed) ')
      console.log(newVal,oldVal)
  }
  }

}
</script>

<template>
<v-app class="main" >
  <div class="width">
    <v-container>
      <p>Об'єкт userObject</p>
      <p>{{userObject}}</p>

      <v-divider></v-divider>

      <p>{{computed}}</p>

      <v-container>
      <input type="text" :value="inputName" placeholder="Введіть ім'я" name="name" @input="inputObject($event)" >
      <input type="number" :value="inputAge" placeholder="Введіть вік" name="age" min="1" @input="inputObject($event)"></v-container>
      <v-sheet v-if="textRule.length > 0" color="red">{{ textRule.join(' ') }}</v-sheet>

      <v-btn @click="changeObject">Змінити значення об'єкту</v-btn>
      <v-divider></v-divider>

      <p>{{combinedData}}</p>

      <v-divider></v-divider>

      <v-checkbox label="Показати об'єкт продуктів в з категорії jewelery" v-model="showJewelery"></v-checkbox>

      <label>Фільтрувати масив по ціні продуктів</label>
      <p>{{price}}</p>
      <v-slider step="5"
                show-ticks="always"
                tick-size="3"
                v-model="price"
                max="100"
                min="5"
      ></v-slider>
      <v-checkbox v-model="checkbox" label="Фільтр через computed properties (від більшої ціни до меншої)"/>
      <p>{{jewelery}}</p>

      <Products :price-limit="price" :products="products" :checkbox="checkbox"/>

    </v-container>
  </div>
</v-app>

</template>

<style scoped>
input {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
.main{
  min-height: 100vh;
  width: 100%;
  margin: 0;
}
.width{
  padding: 2rem;
  text-align: center;
}
.width .select{
  max-width: 400px;
  margin: 0 auto;
}
</style>
