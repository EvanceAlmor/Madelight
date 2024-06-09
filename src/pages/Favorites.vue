<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

import CardList from "../components/CardList.vue";

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      "https://604781a0efa572c1.mokky.dev/favorites?_relations=items"
    );

    favorites.value = data.map((obj) => obj.item);
  } catch (err) {
    console.log(err);
  }
});
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <ul
    class="nav__menu"
    id="menu"
    tabindex="-1"
    aria-label="main navigation"
    hidden
  >
    <li class="nav__item"><a href="#" class="nav__link">Home</a></li>
    <li class="nav__item"><a href="#" class="nav__link">Shop</a></li>
    <li class="nav__item"><a href="#" class="nav__link">Blog</a></li>
    <li class="nav__item"><a href="#" class="nav__link">About</a></li>
    <li class="nav__item"><a href="#" class="nav__link">Contact</a></li>
  </ul>
  <CardList :items="favorites" is-favorites />
</template>
