<template>
  <div id="basketCardCap">
    <div id="basketCardImg">
      <img :src="item.img" />
    </div>
    <div id="basketCardNamePrice">
      <div id="basketCardName">{{ item.name }}</div>
      <div id="basketCardPrice">{{ item.price }} $</div>
    </div>
    <div id="basketCardQuantity">
      <button
        v-if="cardQuantityDelet"
        @click="basketCardDelet"
        id="basketCardDelet"
        class="basketCardAddReduce"
      ><fa icon="trash"></fa></button>
      <button
        v-else
        @click="basketCardReduce"
        id="basketCardReduce"
        class="basketCardAddReduce"
      >-</button>
      <div id="basketCardTotalQuantitly">{{ item.piece }}</div>
      <button
        @click="basketCardAdd"
        id="basketCardAdd"
        class="basketCardAddReduce"
      >+</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      localBasket: JSON.parse(localStorage.getItem("localBasket")),
      cardQuantityDelet: "",
      localBasketStore: [],
    };
  },
  methods: {

    basketCardDelet() {

      this.localBasket.forEach((element) => {
        if (element.id != this.item.id) {
          this.localBasketStore.push(element);
        }
      });

      this.setTimeoutLocalReload();

    },

    basketCardReduce() {

      this.localBasketStore = this.localBasket.map((element) => {

        if (element.id == this.item.id) {

          element.piece > 1
            ? (element.piece = element.piece - 1)
            : (element.piece = 1);

          return element;

        } else return element;

      });

      this.setTimeoutLocalReload();

    },

    basketCardAdd() {

      this.localBasketStore = this.localBasket.map((element) => {

        if (element.id == this.item.id) {

          element.piece = element.piece + 1;
          return element;

        } else return element;

      });

      this.setTimeoutLocalReload();

    },

    setTimeoutLocalReload(){
        setTimeout(() => {
          localStorage.setItem("localBasket",JSON.stringify(this.localBasketStore));
          window.location.reload();
        }, 100);
    }

  },

  created() {
    this.item.piece == 1
      ? (this.cardQuantityDelet = true)
      : (this.cardQuantityDelet = false);
  },

};
</script>
