<template>
  <div id="cardCap">
    <div id="cardImg">
      <img :src="item.img" />
    </div>
    <div id="cardName">{{ item.name }}</div>
    <div id="cardPrice">{{ item.price }} $</div>
    <div v-if="cardQuantityType" @click="cardQuantityPluse" id="cardPluse">+</div>
    <div v-else id="cardQuantity">
      <button
        v-if="cardQuantityDR"
        @click="cardQuantityDelet"
        id="cardQuantityDelet"
        class="cardQuantityAddReduce">
      <fa icon="trash"></fa></button>
      <button
        v-else
        @click="cardQuantityReduce"
        id="cardQuantityReduce"
        class="cardQuantityAddReduce"
      >-</button>
      <div id="cardTotalQuantitly">{{ cardTotalQuantitly }}</div>
      <button
        @click="cardQuantityAdd"
        id="cardQuantityAdd"
        class="cardQuantityAddReduce"
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
      cardQuantityType: true,
      localBasketStore: [],
      cardQuantityDR: "",
      cardTotalQuantitly: "",
    };
  },
  methods: {
    cardQuantityPluse() {

      if (this.localBasket == null || this.localBasket == "") {

        this.localBasketStore.push(this.item);
        this.setTimeoutLocalReload();

      } else {

        this.localBasket.forEach((element) => this.localBasketStore.push(element));
        this.localBasketStore.push(this.item);
        this.setTimeoutLocalReload();

      }

    },
    cardQuantityDelet() {

      this.localBasket.forEach((element) => {
        if (element.id != this.item.id) {
          this.localBasketStore.push(element);
        }
      });

      this.setTimeoutLocalReload();

    },
    cardQuantityAdd() {

      this.localBasketStore = this.localBasket.map((element) => {

        if (element.id == this.item.id) {

          element.piece = element.piece + 1;
          return element;

        } else return element;

      });

      this.setTimeoutLocalReload();

    },
    cardQuantityReduce() {

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
    setTimeoutLocalReload(){
        setTimeout(() => {
          localStorage.setItem("localBasket",JSON.stringify(this.localBasketStore));
          window.location.reload();
        }, 100);
    }
  },
  created() {
      
    if (this.localBasket == null || this.localBasket == "") {
      this.cardQuantityType = true;
    } else {

      this.localBasket.forEach((element) => {

        if (element.id == this.item.id) {

          this.cardQuantityType = false;

          this.cardTotalQuantitly = element.piece;

          element.piece == 1 ? this.cardQuantityDR = true : this.cardQuantityDR = false;

        }

      });

    }

  },
};
</script>
