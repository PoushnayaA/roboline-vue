<template>
  <div class="container">
    <header>
      <h1 class="title">Каталог мороженого</h1>
      <p class="information">Выбери свой вкус</p>
      <div class="cart">
        <div class="cart__title-wrapper">
          <img src="./assets/cart.svg">
          <h2 class="cart__title">Корзина</h2>
        </div>
        <div>Выбрано <span class="cart__count">{{ cartCount }}</span> шт.
          на сумму <span class="cart__count">{{ totalPrice }}</span> ₽</div>
      </div>
    </header>
    <section class="products">
      <h2 class="visually-hidden">Список товаров</h2>
      <button class="products__sort" @click="toggleSort">{{ sortButtonText }}</button>
      <ul class="products__list">
        <product-card v-for="product in sortedProducts" :key="product.id" :product="product"
          @add-to-cart="addToCart"></product-card>
      </ul>
    </section>
    <footer>
      <div class="footer">
        <div class="footer__text">Автор: <span class="bold">Пушная Анастасия</span></div>
        <div class="footer__text">Почта: <a class="bold" href="mailto:poushnaya@bk.ru">poushnaya@bk.ru</a></div>
      </div>
    </footer>
  </div>
</template>

<script>
import { onMounted, ref, computed } from 'vue';
import ProductCard from './components/ProductCard.vue'

export default {
  name: 'App',
  components: {
    ProductCard
  },
  setup() {
    // localStorage.clear();
    const products = ref([]);

    const fetchProducts = async () => {
      const response = await fetch('https://webapi.omoloko.ru/api/v1/products');
      const data = await response.json();
      products.value = data.products;
    };

    const cart = ref([]);

    const cartCount = ref(0);
    const totalPrice = ref(0);

    const addToCart = (productItem) => {
      cart.value.push(productItem);
      localStorage.setItem('cart', JSON.stringify(cart.value));
      updateCartInfo();
    }

    const updateCartInfo = () => {
      const count = cart.value.length;
      const price = cart.value.reduce((sum, item) => sum + item.cost, 0);
      cartCount.value = count;
      totalPrice.value = price;
    };

    onMounted(() => {
      fetchProducts();
      const savedCart = JSON.parse(localStorage.getItem('cart')) || [];
      cart.value = savedCart;
      updateCartInfo();
    });

    const isSorted = ref(false);

    const sortedProducts = computed(() => {
      if (isSorted.value) {
        return [...products.value].sort((a, b) => a.title.localeCompare(b.title));
      }
      return products.value;
    });

    const sortButtonText = computed(() => {
      return isSorted.value ? 'Сортировать по умолчанию' : 'Сортировать по названию';
    });

    const toggleSort = () => {
      isSorted.value = !isSorted.value;
    };


    return { products, addToCart, cartCount, totalPrice, sortedProducts, toggleSort, sortButtonText  }
  }
}
</script>


<style scoped>
.container {
  background-color: #3a5b7d10;
  padding: 20px 16px;
  font-family: 'Montserrat', sans-serif;
}

.title {
  margin: 0;
  text-align: center;
  font-weight: bold;
  color: #3a5b7d;
}

.information {
  margin: 5px 0;
  text-align: center;
  font-weight: 100;
  color: #3a5b7db6;
}

.products__sort {
  border: none;
  background-color: #3a5b7d5b;
  color: #ffffff;
  border-radius: 5px;
  padding: 5px 10px;
  width: max-content;
  display: block;
  margin: 10px 0 10px;
  font-size: 0.8rem;
  font-weight: 400;
  cursor: pointer;
}

.products__sort:hover {
  background-color: #3a5b7d89;
}

.cart {
  display: flex;
  flex-direction: column;
  gap: 8px;
  align-items: end;
  margin: 12px 0 12px auto;
  background-color: #ffffff;
  width: max-content;
  padding: 8px;
  border-radius: 5px;
}

.cart__title-wrapper {
  display: flex;
  align-items: center;
  gap: 8px;
}

.cart__title {
  margin: 0;
  font-size: 1.1rem;
  font-weight: bold;
  color: #3a5b7d;
}

.cart__count {
  font-weight: bold;
  color: #3a5b7d;
}

.products__list {
  list-style-type: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 16px;
  grid-template-columns: repeat(4, 1fr);
}

@media (max-width: 1040px) {
  .products__list {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .products__list {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 500px) {
  .products__list {
    grid-template-columns: repeat(1, 1fr);
  }
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}

.footer {
  margin: 40px 0 20px;
}

.footer__text {
  color: #3a5b7d;
}

.footer__text .bold {
  font-weight: 700;
  color: #3a5b7d;
  text-decoration: none;
}

.footer__text a:hover {
  opacity: 70%;
}
</style>
