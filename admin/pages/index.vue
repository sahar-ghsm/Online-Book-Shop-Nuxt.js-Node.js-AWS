<template>
 <main>
   <div class="a-spacing-large"></div>

   <div class="container-fluid browsing-history">
     <div class="row">
       <div class="col-sm-8 col-8">
         <h1 class="a-size-large a-spacing-none a-text-normal">All Products</h1>
         <div class="a-spacing-large"></div>
         <!-- Button -->
         <nuxt-link to="/product" class="a-button-buy-again">Add a new product</nuxt-link>
         <nuxt-link to="/category" class="a-button-history margin-right-10">Add a new category</nuxt-link>
         <nuxt-link to="/owner" class="a-button-history margin-right-10">Add a new owner</nuxt-link>
        <!-- Listing page -->
       </div>
     </div>
   </div>
<div class="a-spacing-large"></div>
   <div class="container-fluid browsing-history">
     <div class="row">
       <div v-for="(product, index) in products" :key="product._id" class="col-xl-2 col-lg-2 col-md-3 col-sm-6 col-6 br bb">
       <div class="hostory-box">
         <!-- Product image-->
         <a href="#" class=a-link-normal>
           <img :src="product.photo" class="img-fluid">
         </a>
         <!-- Product title-->
        <div class="a-spacing-top-base asin-title">
          <span class="a-text-normal">
            <div class="p13n-sc-truncated">{{product.title}}</div>
          </span>
        </div>
        <!-- Product rating-->
        <div class="a-row">
          <a href="#">
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
            <i class="fas fa-star"></i>
          </a>
          <span class="a-letter-space"></span>
          <span class="a-color-tertiary a-size-small asin-reviews">(1732)</span>
        </div>

        <!-- Product price-->
        <div class="a-row">
          <span class="a-size-base a-color-price">
            <span class="p13n-sc-price">${{product.price}}</span>
          </span>
        </div>

        <!-- Product Buttons-->
        <div class="a-row">
          <nuxt-link :to="`/products/${product._id}`" class="a-button-history margin-right-10">Update</nuxt-link>
          <a href="#" @click="onDeleteProduct(product._id,index)" class="a-button-history margin-right-10">Delete</a>
        </div>
       </div>
     </div>
     </div>
   </div>
 </main> 
</template>

<script>
import Logo from "~/components/Logo.vue"
export default {
  async asyncData({ $axios }){
    try {
      let response = await $axios.$get("http://localhost:3000/api/products")
      console.log(response);
      return {
        products: response.products
      }
    } catch(err) {
      console.log(err)
    }
  },
  methods:{
  async onDeleteProduct(id, index){
    try {
      let response = await this.$axios.$delete(`http://localhost:3000/api/products/${id}`);
      if(response.status){
        this.products.splice(index,1);
      }
    } catch (error) {
      console.log(error)
    }
  }
}
}
</script>

<style>

</style>
