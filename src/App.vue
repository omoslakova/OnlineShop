<template>
  <div class="wrapper">
    <div class="cart">
      <Button
          class="cart__btn-open"
          @click="changeIsCartOpen(true)"
      >
        Open cart ({{ cartTotalNumber }})
      </Button>
      <Cart
          v-if="isCartOpen"
          :key="cart.id"
          :cart="cart"
          @deleteFromCart="removeFromCart"
          @closeCart="changeIsCartOpen(false)"
      />
    </div>
    <div class="products">
      <Product
          v-for="(product) in products"
          :key="product.id"
          :product="product"
          @addProduct="addProduct"
      />
    </div>
  </div>
</template>

<script>

import Cart from './components/Cart.vue';
import Product from './components/Product.vue';
import {onMounted, shallowRef} from "@vue/runtime-core";
import axios from 'axios';
import {computed, ref} from "@vue/reactivity";
import Button from "./components/Button.vue";

export default {
  name: "App",
  components: {
    Button,
    Cart,
    Product,
  },
  emits: ['deleteProduct'],
  setup() {

    const products = shallowRef([]);

    const cart = ref({})

    const isCartOpen = ref(false);

    const addProduct = (product) => {
      if (cart.value.hasOwnProperty(product.id)) {
        cart.value[product.id].count++
      } else {
        cart.value[product.id] = {
          product,
          count: 1
        };
      }
    };

    const cartTotalNumber = computed(() => {
      return Object.keys(cart.value).length;
    });

    const changeIsCartOpen = (newValue) => {
      isCartOpen.value = newValue
    }

    const removeFromCart = (id) => {
      delete cart.value[id]
    }

    onMounted(() => {
      axios
          .get('https://dummyjson.com/products')
          .then(response => {
            products.value = response.data.products
          });
    })

    return {
      products,
      cart,
      addProduct,
      removeFromCart,
      isCartOpen,
      changeIsCartOpen,
      cartTotalNumber,
    };
  },
};

</script>

<style lang="scss" scoped>

.wrapper {
  font-weight: normal;
  color: darkslategray;

  .cart {
    padding: 15px 0;
    font-weight: bold;
    display: flex;

    &__btn-open {
      width: 115px;
      position: relative;
      border: 2px solid black;
    }
  }

  .products {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
    grid-gap: 40px;
  }
}

</style>
