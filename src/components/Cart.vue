<template>
  <dialog
      open
      class="cart-modal"
  >
    <div class="cart-modal__items">
      <CartItem
          v-for="(item) in cart"
          :key="item.id"
          :item="item"
          @changeCartCount="emitChangeCartCount"
          @deleteFromCart="emitDeleteFromCart"
      />
    </div>
    <div class="cart-modal__info info">
      <p class="info__total-price">
        Total: {{ cartTotalCost }} $
      </p>
      <p class="info__total-positions">
        Positions: {{ cartTotalPositions }}
      </p>
      <button
          @click="emitCloseCart"
          class="info__btn-close"
      >
        CLOSE
      </button>
    </div>
  </dialog>
</template>

<script>
import CartItem from './CartItem.vue';
import {computed} from "@vue/reactivity";

export default {
  name: "Cart",
  components: {
    CartItem,
  },
  props: {
    cart: {
      type: Object,
      required: true
    },
  },
  emits: ['changeCartCount', 'deleteFromCart', 'closeCart'],
  setup(props, {emit}) {

    const emitChangeCartCount = () => {
      emit('changeCartCount')
    };

    const emitDeleteFromCart = (id) => {
      emit('deleteFromCart', id)
    };

    const emitCloseCart = () => {
      emit('closeCart')
    };

    const cartTotalPositions = computed(() => {
      return Object.keys(props.cart).length;
    });

    const cartTotalCost = computed(() => {
      return Object.values(props.cart).reduce((sum, cartItem) => {
        const totalPricePerProduct = cartItem.count * cartItem.product.price
        return sum + totalPricePerProduct;
      }, 0);
    });

    return {
      emitChangeCartCount,
      emitDeleteFromCart,
      cartTotalCost,
      emitCloseCart,
      cartTotalPositions,
    };
  },
};

</script>

<style lang="scss" scoped>

.cart-modal {
  width: 900px;
  min-height: 600px;
  position: fixed;
  display: grid;
  grid-template-rows: 1fr auto;

  &__items {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 250px));
  }

  .info {
    display: grid;
    grid-template-columns: repeat(3, auto);
    justify-content: end;
    grid-gap: 40px;
    font-weight: bold;

    &__btn-close {
      background-color: white;
      color: black;
      border: 2px solid black;
      border-radius: 4px;
      width: 100px;
      height: 30px;
      font-weight: bold;
      place-self: center;
    }
  }
}

</style>