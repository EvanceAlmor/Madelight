<!--
  This example requires some changes to your config:
  
  ```
  // tailwind.config.js
  module.exports = {
    // ...
    plugins: [
      // ...
      require('@tailwindcss/forms'),
    ],
  }
  ```
-->
<template>
    <div class="bg-white w-full p-4">
      <div>
        <!-- Mobile filter dialog -->
        <TransitionRoot as="template" :show="mobileFiltersOpen">
          <Dialog class="relative z-40 lg:hidden" @close="mobileFiltersOpen = false">
            <TransitionChild as="template" enter="transition-opacity ease-linear duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="transition-opacity ease-linear duration-300" leave-from="opacity-100" leave-to="opacity-0">
              <div class="fixed inset-0 bg-black bg-opacity-25" />
            </TransitionChild>
  
            <div class="fixed inset-0 z-40 flex">
              <TransitionChild as="template" enter="transition ease-in-out duration-300 transform" enter-from="translate-x-full" enter-to="translate-x-0" leave="transition ease-in-out duration-300 transform" leave-from="translate-x-0" leave-to="translate-x-full">
                <DialogPanel class="relative ml-auto flex h-full w-full max-w-xs flex-col overflow-y-auto bg-white py-4 pb-12 shadow-xl">
                  <div class="flex items-center justify-between px-4">
                    <h2 class="text-lg font-medium text-gray-900">Filters</h2>
                    <button type="button" class="-mr-2 flex h-10 w-10 items-center justify-center rounded-md bg-white p-2 text-gray-400" @click="mobileFiltersOpen = false">
                      <span class="sr-only">Close menu</span>
                      <XMarkIcon class="h-6 w-6" aria-hidden="true" />
                    </button>
                  </div>
  
                  <!-- Filters -->
                  <form class="mt-4 border-t border-gray-200">
                    <h3 class="sr-only">Categories</h3>
                    <ul role="list" class="px-2 py-3 font-medium text-gray-900">
                      <li v-for="category in subCategories" :key="category.name">
                        <a :href="category.href" class="block px-2 py-3">{{ category.name }}</a>
                      </li>
                    </ul>
  
                    <Disclosure as="div" v-for="section in filtersColor" :key="section.id" class="border-t border-gray-200 px-4 py-6" v-slot="{ open }">
                      <h3 class="-mx-2 -my-3 flow-root">
                        <DisclosureButton class="flex w-full items-center justify-between bg-white px-2 py-3 text-gray-400 hover:text-gray-500">
                          <span class="font-medium text-gray-900">{{ section.name }}</span>
                          <span class="ml-6 flex items-center">
                            <PlusIcon v-if="!open" class="h-5 w-5" aria-hidden="true" />
                            <MinusIcon v-else class="h-5 w-5" aria-hidden="true" />
                          </span>
                        </DisclosureButton>
                      </h3>
                      <DisclosurePanel class="pt-6">
                        <div class="space-y-6">
                          <div v-for="(option, optionIdx) in section.options" :key="option.value" class="flex items-center">
                            <input :id="`filter-mobile-${section.id}-${optionIdx}`" :name="`${section.id}[]`" :value="option.value" type="checkbox" :checked="option.checked" class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                            <label :for="`filter-mobile-${section.id}-${optionIdx}`" class="ml-3 min-w-0 flex-1 text-gray-500">{{ option.label }}</label>
                          </div>
                        </div>
                      </DisclosurePanel>
                    </Disclosure>
                  </form>
                </DialogPanel>
              </TransitionChild>
            </div>
          </Dialog>
        </TransitionRoot>
       
     
     <select
       @change="onChangeSelect"
       class="w-40 py-2 px-4 border border-slate-300 rounded-md outline-none hidden"
     >
       <option value="name">По названию</option>
       <option value="price">По цене (дешевые)</option>
       <option value="-price">По цене (дорогие)</option>
     </select>
   
     <select
       @change="onChangeSelect"
       class="py-2 px-3 border rounded-md outline-none hidden"
     >
       <option value="name">По названию</option>
       <option value="price">По цене (дешевые)</option>
       <option value="-price">По цене (дорогие)</option>
     </select>
       <!-- <img class="absolute left-10 top-28" src="/search.svg" /> -->
       <input
         @input="onChangeSearchInput"
         class="border rounded-md w-full outline-none focus:border-gray-400"
         type="text"
         placeholder="Поиск..."
       />
        <main class=" w-full sm:px-6 ">
          <div class="flex items-baseline justify-between border-b border-gray-200 pb-6 pt-4">
            <h1 class="text-4xl font-bold tracking-tight text-gray-900">Товары</h1>
            <div class="bg-white flex justify-between items-center">
  

    
  </div>
  <!-- <div class="md:hidden w-full">
    <li class="flex list-none">
        <img class="w-8" src="/filter.svg" alt="" />
        <b class="pl-2 text-xl m-auto">Фильтры</b>
      </li>
    <div class="w-full relative">
      <img class="absolute left-4 top-7" src="/search.svg" />
      <input
        @input="onChangeSearchInput"
        class="w-full border mt-4 rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
        type="text"
        placeholder="Поиск..."
      />
    </div>
  </div> -->
            <div class="flex items-center">
              <Menu as="div" class="relative inline-block text-left">
                <div>
                  <MenuButton class="group inline-flex justify-center text-sm font-medium text-gray-700 hover:text-gray-900">
                    Сорт.
                    <ChevronDownIcon class="-mr-1 ml-1 h-5 w-5 flex-shrink-0 text-gray-400 group-hover:text-gray-500" aria-hidden="true" />
                  </MenuButton>
                </div>
  
                <transition enter-active-class="transition ease-out duration-100" enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100" leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
                  <MenuItems class="absolute right-0 z-10 mt-2 w-40 origin-top-right rounded-md bg-white shadow-2xl ring-1 ring-black ring-opacity-5 focus:outline-none">
                    <div class="py-1">
                      <MenuItem v-for="option in sortOptions" :key="option.name" v-slot="{ active }">
                        <a :href="option.href" :class="[option.current ? 'font-medium text-gray-900' : 'text-gray-500', active ? 'bg-gray-100' : '', 'block px-4 py-2 text-sm']">{{ option.name }}</a>
                      </MenuItem>
                    </div>
                  </MenuItems>
                </transition>
              </Menu>
  
              <button type="button" class="-m-2 ml-5 p-2 text-gray-400 hover:text-gray-500 sm:ml-7">
                <span class="sr-only">View grid</span>
                <Squares2X2Icon class="h-5 w-5" aria-hidden="true" />
              </button>
              <button type="button" class="-m-2 ml-4 p-2 text-gray-400 hover:text-gray-500 sm:ml-6 lg:hidden" @click="mobileFiltersOpen = true">
                <span class="sr-only">Filters</span>
                <FunnelIcon class="h-5 w-5" aria-hidden="true" />
              </button>
            </div>
          </div>
  
          <section aria-labelledby="products-heading" class="pb-24 pt-6">
            <h2 id="products-heading" class="sr-only">Products</h2>
  
            <div class="grid grid-cols-1 gap-x-8 gap-y-10 lg:grid-cols-4">
              <!-- Filters -->
              <form class="hidden lg:block">
                <h3 class="sr-only">Категории</h3>
                <ul role="list" class="space-y-4 border-b border-gray-200 pb-6 text-sm font-medium text-gray-900">
                  <li v-for="category in subCategories" :key="category.name">
                    <a :href="category.href">{{ category.name }}</a>
                  </li>
                </ul>
  
                <Disclosure as="div" v-for="section in filtersColor" :key="section.id" class="border-b border-gray-200 py-6" v-slot="{ open }">
                  <h3 class="-my-3 flow-root">
                    <DisclosureButton class="flex w-full items-center justify-between bg-white py-3 text-sm text-gray-400 hover:text-gray-500">
                      <span class="font-medium text-gray-900">{{ section.name }}</span>
                      <span class="ml-6 flex items-center">
                        <PlusIcon v-if="!open" class="h-5 w-5" aria-hidden="true" />
                        <MinusIcon v-else class="h-5 w-5" aria-hidden="true" />
                      </span>
                    </DisclosureButton>
                  </h3>
                  <DisclosurePanel class="pt-6">
                    <div class="space-y-4">
                      <div v-for="(option, optionIdx) in section.options" :key="option.value" class="flex items-center">
                        <input :id="`filter-${section.id}-${optionIdx}`" :name="`${section.id}[]`" :value="option.value" type="checkbox" :checked="option.checked" class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                        <label :for="`filter-${section.id}-${optionIdx}`" class="ml-3 text-sm text-gray-600">{{ option.label }}</label>
                      </div>
                    </div>
                  </DisclosurePanel>
                </Disclosure>
              </form>
  
              <!-- Product grid -->
              <div class="lg:col-span-3">
              <div class="mt-10">
    <CardList
      :items="items"
      @add-to-favorite="addToFavorite"
      @add-to-cart="onClickAddPlus"
    />
  </div>
              </div>
            </div>
          </section>
        </main>
      </div>
    </div>
  </template>
  
  <script setup>
  import { reactive, watch, ref, onMounted } from "vue";
import axios from "axios";
import debounce from "lodash.debounce";
import { inject } from "vue";
import CardList from "../components/CardList.vue";
import Slider from "../components/Slider.vue";
const { cart, addToCart, removeFromCart } = inject("cart");
import { Popover, PopoverButton, PopoverPanel } from "@headlessui/vue";
import {
  ChevronDownIcon,
  PhoneIcon,
  PlayCircleIcon,
} from "@heroicons/vue/20/solid";
import Filter from '../components/Filter.vue'
import {
  ArrowPathIcon,
  ChartPieIcon,
  CursorArrowRaysIcon,
  FingerPrintIcon,
  SquaresPlusIcon,
} from "@heroicons/vue/24/outline";
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
  
  import {
    Dialog,
    DialogPanel,
    Disclosure,
    DisclosureButton,
    DisclosurePanel,
    Menu,
    MenuButton,
    MenuItem,
    MenuItems,
    TransitionChild,
    TransitionRoot,
  } from '@headlessui/vue'
  import { XMarkIcon } from '@heroicons/vue/24/outline'
  import { FunnelIcon, MinusIcon, PlusIcon, Squares2X2Icon } from '@heroicons/vue/20/solid'
  
  const sortOptions = [
    { name: 'Популярное', href: '#', current: true },
    { name: 'Лучшие отзывы', href: '#', current: false },
    { name: 'Новое', href: '#', current: false },
    { name: 'Сначала дешевле', href: '#', current: false },
    { name: 'Сначала дороже', href: '#', current: false },
  ]
  const subCategories = [
    { name: 'Свечи формовые', href: '#' },
    { name: 'Свечи контейнерные', href: '#' },
    { name: 'Гипсовые изделия', href: '#' },
    { name: 'Интерьерные наборы', href: '#' },
    { name: 'Подарочные наборы', href: '#' },
  ]
  const filtersColor = [
    {
      id: 'color',
      name: 'Цвета',
      options: [
        { value: 'white', label: 'White', checked: false },
        { value: 'beige', label: 'Beige', checked: false },
        { value: 'blue', label: 'Blue', checked: true },
        { value: 'brown', label: 'Brown', checked: false },
        { value: 'green', label: 'Green', checked: false },
        { value: 'purple', label: 'Purple', checked: false },
      ],
    },
    {
      id: 'category',
      name: 'Категории',
      options: [
        { value: 'Свечи формовые', label: 'Свечи формовые', checked: false },
        { value: 'Свечи контейнерные', label: 'Свечи контейнерные', checked: false },
        { value: 'Гипсовые изделия', label: 'Гипсовые изделия', checked: true },
        { value: 'Интерьерные наборы', label: 'Интерьерные наборы', checked: false },
        { value: 'Подарочные наборы', label: 'Подарочные наборы', checked: false },
      ],
    },
    {
      id: 'size',
      name: 'Размеры',
      options: [
        { value: '2l', label: '2L', checked: false },
        { value: '6l', label: '6L', checked: false },
        { value: '12l', label: '12L', checked: false },
        { value: '18l', label: '18L', checked: false },
        { value: '20l', label: '20L', checked: false },
        { value: '40l', label: '40L', checked: true },
      ],
    },
  ]
  
  const mobileFiltersOpen = ref(false)
  </script>