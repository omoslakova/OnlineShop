<template>
  <div class="product-item">
    <img
        :src="product.images[0]"
        :alt="product.title"
        class="product-item__img"
    >
    <div class="product-item__info info">
      <p class="info__product-title">
        {{ product.title }}
      </p>
      <p class="info__product-description">
      {{ product.description }}
      </p>
      <p class="info__product-price">
          Price: {{ product.price }} $
      </p>
      <Button
          class="info__btn-add"
          @click="emitAddProduct"
      >
        Add to cart
      </Button>
    </div>
  </div>
</template>

<script>
import Button from './Button.vue';

export default {
  name: "Product",
  components: {Button},
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
  emits: ['addProduct'],
  setup(props, {emit}) {

    const emitAddProduct = () => {
      emit('addProduct', props.product);
    }

    return {
      emitAddProduct,
    };
  },
};

</script>

<style lang="scss" scoped>

.product-item {
  border-radius: 6px;
  padding: 25px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  box-shadow: 0 0 8px 0 #e0e0e0;

  &:hover {
    box-shadow: 0 0 8px 0 teal;
  }

  &__img {
    width: 100%;
    height: 250px;
    object-fit: contain;
  }

  .info {
    display: grid;
    grid-template-rows: 60px 90px 70px 40px;
    padding: 15px;
    justify-items: center;

    &__product-title {
      font-weight: bold;
      font-size: 20px;
    }

    &__product-description {
      font-size: 17px;
    }

    &__product-price {
      font-weight: bolder;
      color: teal;
      font-size: 18px;
    }

    &__btn-add {
      border: 2px solid #e0e0e0;
      width: 140px;

      &:hover {
        box-shadow: 0 0 4px 0 teal;
      }
    }
  }
}

</style>