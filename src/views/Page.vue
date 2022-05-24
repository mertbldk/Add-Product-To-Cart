<template>
  <div id="container">
    <div id="basketContainer">
      <div id="basketTitle">
        <div id="basketMyBasket">My Basket</div>
        <button @click="basketClearCart" id="basketClearCart">Clear Cart</button>
      </div>
      <div id="basketMain">
        <div v-if="localBasket">
          <BasketCard v-for="item in localBasket" :key="item.id" :item="item" />
        </div>
        <div v-else id="BasketİsEmpty">Basket İs Empty !</div>
      </div>
      <div id="basketFooter">
        <div id="basketPrice">{{ basketTotal }} $</div>
        <button id="basketGoToCart">Go To Cart</button>
      </div>
    </div>
    <div id="cardContainer">
      <ProductCard v-for="item in Products" :key="item.id" :item="item" />
    </div>
  </div>
</template>

<script>
import ProductCard from "../components/ProductCard.vue";
import BasketCard from "../components/BasketCard.vue";

export default {
    components: {
      ProductCard,
      BasketCard,
    },
    data() {
      return {
        localBasket:
        JSON.parse(localStorage.getItem("localBasket")) == null ||
        JSON.parse(localStorage.getItem("localBasket")) == ""
          ? false
          : JSON.parse(localStorage.getItem("localBasket")),
        basketTotal: 0,
        Products : ''
      };
    },
    methods: {
      basketClearCart() {
        localStorage.removeItem("localBasket");
        setTimeout(window.location.reload(), 100);
      },
    },
    created() {

      if (this.localBasket != null && this.localBasket != "") this.localBasket.forEach((element) => (this.basketTotal += element.price * element.piece));

      this.Products = this.$store.state.Products;
    
    },
};
</script>
