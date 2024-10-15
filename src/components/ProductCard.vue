<template>
  <li class="products__item">
    <a class="products__link" href="#">
      <h3 class="products__title">{{ product.title }}</h3>
      <img class="products__img" :src="product.image">
      <div class="products__wrapper">
        <div class="products__cost">Цена: {{ product.cost }} ₽</div>
        <div class="products__cost products__cost--by-card">Цена по карте: {{ product.costByCard }} ₽</div>
        <button class="products__button" @click="$emit('add-to-cart', product)">Добавить в корзину</button>
      </div>
    </a>
  </li>
</template>

<script>
export default {
  name: 'ProductCard',
  props: {
    product: Object,
  }
}
</script>

<style scoped>

.products__item {
height: 100%;
}

.products__link {
  text-decoration: none;
  border: 1px solid #3a5b7d45;
  border-radius: 5px;
  padding: 10px;
  background-color: #ffffff;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  box-sizing: border-box;
}

.products__item:hover {
  transform: scale(102%);
  cursor: pointer;
}

.products__title {
  font-size: 1rem;
  font-weight: bold;
  color: #3a5b7dbe;
  margin-top: 0;
  text-align: center;
}

.products__img {
  width: 100%;
  max-height: 220px;
  object-fit: cover;
}

.products__cost {
  margin: 6px 0;
  color: #3a5b7dbe;
  font-size: 1rem;
  font-weight: 400;
  text-align: right;
}

.products__cost--by-card {
  color: #3a5b7d;
  font-weight: 1000;
}

.products__button {
  border: none;
  background-color: #3a5b7dbe;
  color: #ffffff;
  border-radius: 5px;
  padding: 10px;
  width: max-content;
  display: block;
  margin: 10px 0 0 auto;
  font-size: 1rem;
  font-weight: 200;
  cursor: pointer;
}

.products__button:hover {
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.4);
}
</style>
