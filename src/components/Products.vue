<script>
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
      filterType: null
    }
  },
  methods:{
    changeState(){
      this.filterType = this.checkbox ? 'computed' : 'method';
    },
    filterMethod(){
      console.log("Фільтр через метод")
     return this.products.filter(product => product.price < this.priceLimit)
    },
  },
computed:{
  toggleFilter() {
    if (this.filterType){
      return this.filterType === 'method' ? this.filterMethod() : this.sortFilter;
    }
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
    <v-btn @click="changeState">Фільтрувати продукти</v-btn>
    <v-row>
      <v-col v-for="product in toggleFilter" :key="product.id" >
        <Product :product="product" />
      </v-col>
    </v-row>
  </v-container>

</template>

<style scoped>

</style>