<template>

  <div class="container pt-2">

    <header class='d-flex justify-content-between'>

      <h1 @click="showCart = false">{{title}}</h1>

      <div @click='showCart = !showCart' class='cart fs-2'>
        <i class="bi bi-cart4"></i>
        ({{totalQuantity}})
        {{totalCost.toFixed(2)}}$
      </div>

    </header>

    <main class='mt-3'>

      <div v-if='!showCart' class='goods-list'>
        
        <div class="row row-cols-4">

          <OneProduct 
            v-for="item in goods"
            :key="item.id" 
            :product="item" 
            @add-to-cart="addToCart"
          />

        </div>

      </div>

      <div v-else class='cart-list border p-5 pt-1'>

        <h2 class='text-end'>
          <i @click="showCart=false" class="bi fs-1 bi-x-square"></i>
        </h2>
        
        <h2 class='my-4'>Your Products In Cart</h2>

        <div 
          v-for="item in goodsInCart"
          :key="item.id" 
          class="row row-cols-5 align-items-center border my-3 p-3 bg-light"
        >
          <h3 class='text-center'>{{item.title}}</h3>
          <h3>Price: {{item.price.toFixed(2)}}$</h3>
          <input min='1' class='fs-3' type="number" v-model="item.qt">
          <h3 class='text-center'>Cost: {{(item.price * item.qt).toFixed(2)}}$</h3>
          <button 
            class='btn btn-danger'
            @click="removeFromCart(item)"
          >
            Remove
          </button>

        </div>

        <h2 class='text-end'>
          Total Cost {{totalCost.toFixed(2)}} $  
          <button @click='clearAll' class='btn btn-danger'>Clear All</button>
        </h2>
        
      </div>

    </main>

  </div>

</template>

<script>

  const URL = 'https://fakestoreapi.com/products/';

  import OneProduct from './components/OneProduct.vue';

  export default {
    name: 'App',
    data(){
      return {
        title: 'Vue Shop App',
        goods: [],
        showCart: false
      }
    },
    computed:{
      goodsInCart(){
        return this.goods.filter(item => item.qt);
      },
      totalQuantity(){
        return this.goods.reduce((acc, item) => item.qt + acc, 0)
      },
      totalCost(){
        return this.goods.reduce((acc, item)=> acc + item.price * item.qt, 0);
      }
    },
    methods:{
      addToCart(id){
        let product = this.goods.find(item => item.id == id);
        
        product.qt = 1;
        
      },
      clearAll(){
        for(let item of this.goodsInCart){
          item.qt = 0;
        }
      },
      removeFromCart(product){

        if(confirm('Are You Sure?!?!')){
          product.qt = 0;
        }

      }
    },
    components: {
      OneProduct
    },
    async mounted(){
      
      let result = await fetch(URL);
          result = await result.json();

          result = result.map(item => ({...item, qt:0}));
    
      console.log(result);

      this.goods = result;

    }
  }

</script>

<style>

</style>
