<template>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Static Template</title>
    <!-- optional CSS framework -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">

  </head>
  <body>
    <!-- Your HTML -->
    <h1>My Site</h1>
    <div class="container text-center">
      <div class="row">
        <div class="col">
          <h1>Search Stuff</h1>
          <div>
            <label for="InputSearch" class="form-label">Chercher un produit par nom :</label>
            <input v-model="search" type="text" class="form-control" id="InputSearch" aria-describedby="emailHelp">
          </div>
          <div class="mb-3 form-check">
            <input v-model="searchByAvailable" type="checkbox" class="form-check-input" id="InputIsAvailable">
            <label class="form-check-label" for="InputIsAvailable">Chercher seulement produit dispo</label>
          </div>
          <select v-model="orderType" class="form-select" aria-label="Default select example">
            <option value="name" selected>Name</option>
            <option value="price">Price</option>
          </select>

        </div>



        <div class="col">
          <h1>Add stuff</h1>
          <div class="input-group">
            <input v-model="newName" type="text" class="form-control" placeholder="Name">
            <input v-model="newPrice" type="number" class="form-control" placeholder="Price">
            <span class="input-group-text">CHF</span>
          </div>
          <div class="mb-3 form-check">
            <input v-model="newIsAvailable" type="checkbox" class="form-check-input" id="InputIsAvailable">
            <label class="form-check-label" for="InputIsAvailable">Is this product available ?</label>
          </div>
          <button @click="addToList" class="btn btn-primary">Add</button>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div v-for="product in listByOrdered" v-bind:key="product.id">
            <ProductComp @addToCart="addToCart(product)" :prod="product"></ProductComp>
            
          </div>
        </div>
        <div class="col">
          <div v-for="cartItem in this.cart" v-bind:key="cartItem.id">
            <CartItem :cartItem="cartItem" @removeFromCart="removeFromCart(cartItem)"></CartItem>
          </div>
        </div>
      </div>
    </div>
    
    

    
  </body>
</template>

<script>
import ProductComp from "/src/components/ProductComp.vue"
import CartItem from "/src/components/CartItem.vue"
import { nanoid } from 'nanoid'
//import { axios } from "axios";
//simply adding axios crashes the website ?
export default {
  components: {
    ProductComp, 
    CartItem
  },
  data() {
    return {
      newName: '',
      newPrice:0,
      newIsAvailable:false,
      search:'',
      searchByAvailable:false,
      orderType:"Name",
      list: [
        {name:'Iphone', id: nanoid(), price:1100, isAvailable:true},
        {name:'Samsung Galaxy', id: nanoid(), price:999, isAvailable:true},
        {name:'Surface Duo', id: nanoid(), price:1399, isAvailable:false}
      ],
      cart:new Map,
      //Using a map probably isn't the best way to do it, but it works well enough I think
    }
  },
  mounted(){
    //this.fetchImage();
  },
  methods: {
    // async fetchImage(){
    //   for (const product of this.list){
    //     const results = await axios.get('https://dummyjson.com/products/search?q=' + product.name);
    //     this.image = results.data.concat(this.image)
    //   }
    // },
    addToList() {
      if (this.newName != '' && this.newPrice > 0 ) {
        this.list.push({name:this.newName, 
                        id: nanoid(), 
                        price:this.newPrice, 
                        isAvailable:this.newIsAvailable});
        this.newIsAvailable = false;
        this.newName = '';
        this.newPrice = 0;
      }
    },
    filterByName(product){
      if (product.name.toUpperCase().includes(this.search.toUpperCase())){
        return true;
      } else return false;
    },
    filterByAvailable(product){
      if (product.isAvailable === true){
        return true;
      } else return false;
    },
    addToCart(product){
      if (!this.cart.has(product)){
        this.cart.set(product, 1)
      }
      else {
        this.cart.set(product, this.cart.get(product) + 1)
      }
    },
    removeFromCart(product){
      if (this.cart.get(product) === 1){
        this.cart.delete(product);
      }
      else {
        this.cart.set(product, this.cart.get(product) -  1)
      }
    }
  },
  computed :{
    listByIsAvailable() {
      if (this.searchByAvailable){
        var copyCat = [];
        copyCat = this.list.filter(this.filterByAvailable)
        return copyCat;
      } else return this.list;
    },
    listBySearch(){
      var copyCat = [];
      copyCat = this.listByIsAvailable.filter(this.filterByName);
      return copyCat;
    },
    listByOrdered(){
      const copyCat = this.listBySearch.slice();
      if (this.orderType === "name"){
        copyCat.sort((a, b) => b.name.toUpperCase().localeCompare(a.name.toUpperCase()));
      } else {
        copyCat.sort((a,b) => b.price - a.price);
      }
      return copyCat;
    },
    
    
    
  }
};
</script>
<style>
/* optional your CSS */
</style>