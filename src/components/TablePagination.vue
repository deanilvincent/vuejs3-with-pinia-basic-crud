<template>
  <Spinner v-if="isLoading" />
  <div v-else>
    <div class="flex justify-end">
      <div>
        <select
          id="countries"
          class="mr-3 bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-100 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 mb-3"
          v-model="perPage"
          :disabled="isLoading"
        >
          <option value="5">5 Per Page</option>
          <option value="10">10 Per Page</option>
          <option value="30">30 Per Page</option>
          <option value="50">50 Per Page</option>
        </select>
      </div>
      <div>
        <div class="flex items-center">
          <label for="simple-search" class="sr-only">Search</label>
          <div class="relative w-full">
            <div
              class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none"
            >
              <svg
                aria-hidden="true"
                class="w-5 h-5 text-gray-500 dark:text-gray-400"
                fill="currentColor"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                  clip-rule="evenodd"
                ></path>
              </svg>
            </div>
            <input
              type="text"
              id="simple-search"
              class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full pl-10 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              placeholder="Search here"
              v-model="search"
              @keyup.enter="handleSearchButton"
            />
          </div>
          <button
            type="button"
            class="p-2.5 ml-2 text-sm font-medium text-white bg-blue-700 rounded-lg border border-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
            @click="handleSearchButton"
          >
            <svg
              class="w-5 h-5"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
              ></path>
            </svg>
            <span class="sr-only">Search</span>
          </button>
        </div>
      </div>
    </div>
    <div>
      <div class="overflow-x-auto relative">
        <table
          class="w-full text-sm text-left text-gray-500 dark:text-gray-400"
        >
          <thead
            class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
          >
            <slot name="header" />
          </thead>
          <tbody>
            <slot name="list" />
          </tbody>
        </table>
      </div>
      <div class="flex justify-end mt-3">
        <nav aria-label="Page navigation example">
          <ul
            class="inline-flex items-center -space-x-px"
            v-for="pageNum in data.totalPages"
            :key="pageNum"
          >
            <li>
              <button
                @click="navigateRoute(pageNum)"
                :class="paginationTabClass(pageNum)"
              >
                {{ pageNum }}
              </button>
            </li>
            <!-- <li v-else-if="pageNum > 2">
          <span class="ml-3 mr-3 text-gray-500">...</span>
        </li> -->
          </ul>
        </nav>
        <button
          class="inline-flex items-center px-4 py-2 ml-1 mr-1 text-sm font-medium text-gray-500 bg-white border border-gray-300 rounded-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
          @click="prevPage()"
        >
          <svg
            aria-hidden="true"
            class="w-5 h-5"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M7.707 14.707a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l2.293 2.293a1 1 0 010 1.414z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
        <button
          class="inline-flex items-center px-4 py-2 text-sm font-medium text-gray-500 bg-white border border-gray-300 rounded-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
          @click="nextPage()"
        >
          <svg
            aria-hidden="true"
            class="w-5 h-5"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useRouter, useRoute } from "vue-router";
import { defaultPaginationTab, activePaginationTab } from "../constants/form";
import { watch, ref } from "vue";
import Spinner from "./Spinner.vue";

const props = defineProps({
  data: {},
  page: null,
  perPage: null,
  paginationRoute: null,
  isLoading: Boolean,
  onHandleSearch: null,
});
const emit = defineEmits(["onHandleSearch"]);

const router = useRouter();
const route = useRoute();

const perPage = ref(props.perPage);
const search = ref("");

const paginationTabClass = (pageNum) => {
  if (pageNum === props.data.pageIndex) return activePaginationTab;
  return defaultPaginationTab;
};

const navigateRoute = (selectedPage) => {
  if (selectedPage) {
    navRoute(selectedPage, perPage.value);
  }
};

const prevPage = () => {
  let selectedPage = Number(route.params.page) - 1;
  selectedPage = selectedPage < 1 ? 1 : selectedPage;
  if (selectedPage) {
    navRoute(selectedPage, perPage.value);
  }
};

const nextPage = () => {
  let selectedPage = Number(route.params.page) + 1;
  if (props.data.totalPages >= selectedPage) {
    navRoute(selectedPage, perPage.value);
  }
};

const handleSearchButton = () => {
  // default to page 1 when search is trigger
  navRoute(1, perPage.value);
  emit("onHandleSearch", search.value);
};

const navRoute = (page, perPage) => {
  router.push({
    name: props.paginationRoute,
    params: {
      page: page,
      perPage: perPage,
    },
  });
};

watch(perPage, (newValue, oldValue) => {
  if (newValue) {
    navRoute(1, newValue);
  }
});

watch(search, (newValue, oldValue) => {
  if (!search.value) {
    handleSearchButton();
  }
});
</script>
