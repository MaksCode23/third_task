<script>
import axiosInstance from "../../services/axios.js";
import product from "@/components/Product.vue";
import Product from "@/components/Product.vue";
export default {
  name: "Products",
  props: {
    priceLimit: Number,
    products: Object,
    checkbox: Boolean
  },
  components:{
    Product
  },
  data(){
    return{
    }
  },
  methods:{
    filterMethod(){
      console.log("Фільтр через метод")
      return this.products.filter(product => product.price < this.priceLimit)
  },
  },
computed:{
    toggleFilter(){
      return this.checkbox ? this.sortFilter : this.filterMethod()
    },
  sortFilter(){
      return this.filterComputed.sort((a, b) => b.price - a.price);
  },
    filterComputed(){
      console.log("Фільтр через computed property")
      return this.products.filter(product => product.price < this.priceLimit)
    }
},

}
</script>

<template>
  <v-container>
    <v-row>
      <v-col v-for="product in toggleFilter" :key="product.id" >
        <Product :product="product" />
      </v-col>
    </v-row>
  </v-container>

</template>

<style scoped>

</style>