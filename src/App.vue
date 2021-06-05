<template>
  <div id="app">
    <AppHearder />
    <b-container>
      <b-row class="justify-content-center">
        <AddProduct @addProduct="addProduct" />
        <ListProduct :products="productList" @deleteProduct="deleteProduct" @updateProduct="updateProduct" />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import AppHearder from './components/AppHeader.vue'
import AddProduct from './components/AddProduct.vue'
import ListProduct from './components/ListProduct.vue'
import axios from 'axios'
export default {
  name: 'App',
  components:{
    AppHearder,
    AddProduct,
    ListProduct
  },
  data(){
    return{
      productList:[]
    }
  },
  methods:{
    async getProductList(){
      try{
        let result = await axios.get('http://localhost:3000/products')
        this.productList = result.data
        console.log('Result ',result.data)
      }catch(error){
        console.log('Error ',error)
      }
      
    },
    async addProduct(newProduct){
      try{
        await axios.post('http://localhost:3000/products',newProduct);
        this.getProductList()
      }catch(error){
        console.log(error)
      }
      console.log('New Product',newProduct)
    },
    async deleteProduct(productId){
      try{
        await axios.delete(`http://localhost:3000/products/${productId}`)
        this.getProductList()
      }catch(error){
        console.log(error)
      }
      console.log("product Id ",productId)
    },
    async updateProduct(updatedProduct){
      try{
        await axios.put(`http://localhost:3000/products/${updatedProduct.id}`,updatedProduct)
        this.getProductList()
      }catch(error){
        console.log(error)
      }
    }
  },
  mounted(){
    this.getProductList();
  }
}
</script>
<style>
  .error-message{
    color: rgb(255, 0, 0);
  }
</style>