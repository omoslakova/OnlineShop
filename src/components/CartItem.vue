<template>
  <div class="cart-item">
    <p class="cart-item__title">
      Name: {{ product.title }}
    </p>
    <img
        :src="product.images[0]"
        :alt="product.title"
        class="cart-item__img"
    >
    <p class="cart-item__price">
      Price: {{ product.price }}
    </p>
    <div class="cart-item__quantity">
      Quantity:
      <div>
        <span @click="changeCartCount(-1)">
        -
        </span>
           {{ count }}
        <span @click="changeCartCount(1)">
        +
        </span>
      </div>
    </div>
    <Button
        class="cart-item__btn-delete"
        @click="emitDeleteItemFromCart"
    >
      DELETE
    </Button>
  </div>
</template>

<script>
import {toRefs, watch} from 'vue';
import Button from "./Button.vue";

export default {
  name: "CartItem",
  components: {Button},
  props: {
    item: {
      type: Object,
      required: true
    },
  },
  emits: ['changeCartCount', 'deleteFromCart'],
  setup(props, {emit}) {

    const {product, count} = toRefs(props.item);

    const emitChangeCartCount = () => {
        emit('changeCartCount')
    }

    const emitDeleteItemFromCart = () => {
      emit('deleteFromCart', props.item.product.id);
    };

    watch(
        () => count.value,
        (newCountValue) => {
        console.log('watch count', newCountValue);
        if (newCountValue === 0) {
          emitDeleteItemFromCart();
        }
        console.log(props.item.product.id);
    });

    const changeCartCount = (value) => {
      count.value += value;
    }

    return {
      product,
      count,
      changeCartCount,
      emitChangeCartCount,
      emitDeleteItemFromCart,
    };
  },
};

</script>

<style lang="scss" scoped>

.cart-item {
  text-align: center;

  &__quantity{
    display: grid;
    grid-template-rows: repeat(3, auto);
    gap: 8px;
  }

  &__title {
    font-weight: bold;
  }

  &__img {
    width: 150px;
    height: 100px;
    object-fit: contain;
  }

  &__price {
    font-weight: bolder;
    color: teal;
  }

  &__btn-delete {
    width: 100px;

    &:hover {
      box-shadow: 0 0 4px 0 teal;
    }
  }
}

</style>