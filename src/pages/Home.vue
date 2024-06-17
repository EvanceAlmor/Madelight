<script setup>
import { reactive, watch, ref, onMounted } from "vue";
import axios from "axios";
import debounce from "lodash.debounce";
import { inject } from "vue";
import CardList from "../components/CardList.vue";
import Slider from "../components/Slider.vue";
const { cart, addToCart, removeFromCart } = inject("cart");

const items = ref([]);

const filters = reactive({
  sortBy: "title",
  searchQuery: "",
});

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value;
}, 300);

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
      };

      item.isFavorite = true;

      const { data } = await axios.post(
        `https://604781a0efa572c1.mokky.dev/favorites`,
        obj
      );

      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(
        `https://604781a0efa572c1.mokky.dev/favorites/${item.favoriteId}`
      );
      item.favoriteId = null;
    }
  } catch (err) {
    console.log(err);
  }
};

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://604781a0efa572c1.mokky.dev/favorites`
    );

    items.value = items.value.map((item) => {
      const favorite = favorites.find(
        (favorite) => favorite.item_id === item.id
      );

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      };
    });
  } catch (err) {
    console.log(err);
  }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const { data } = await axios.get(
      `https://604781a0efa572c1.mokky.dev/items`,
      {
        params,
      }
    );

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  } catch (err) {
    console.log(err);
  }
};

onMounted(async () => {
  const localCart = localStorage.getItem("cart");
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
  }));
});

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false,
  }));
});

watch(filters, fetchItems);
</script>

<template>
  <Slider />
  <div class="bg-white flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-auto">Товары</h2>
    <div class="md:hidden right-4">
      <select
        @change="onChangeSelect"
        class="w-40 py-2 px-4 border border-slate-300 rounded-md outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>
    </div>
    <div class="hidden md:flex gap-4">
      <select
        @change="onChangeSelect"
        class="py-2 px-3 border rounded-md outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" />
        <input
          @input="onChangeSearchInput"
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          type="text"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>
  <div class="md:hidden w-full">
    <!-- <li class="flex list-none">
        <img class="w-8" src="/filter.svg" alt="" />
        <b class="pl-2 text-xl m-auto">Фильтры</b>
      </li> -->
    <div class="w-full relative">
      <img class="absolute left-4 top-7" src="/search.svg" />
      <input
        @input="onChangeSearchInput"
        class="w-full border mt-4 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
        type="text"
        placeholder="Поиск..."
      />
    </div>
  </div>

  <div class="mt-10">
    <CardList
      :items="items"
      @add-to-favorite="addToFavorite"
      @add-to-cart="onClickAddPlus"
    />
  </div>
</template>
