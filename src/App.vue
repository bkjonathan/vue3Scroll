<template>
  <h1>My Blog</h1>
  <div class="input-container">
    <input
      type="text"
      id="filter"
      class="filter"
      placeholder="Filter Post"
      v-model="state.search"
    />
  </div>
  <div id="post-container">
    <Posts v-for="(item, index) in filterItems" :key="index" :post="item" />
    <Loader :show="state.show" />
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */

import Loader from "./components/Loader";
import { ref, reactive, onMounted,computed } from "vue";
import Posts from "./components/Posts";
export default {
  name: "App",
  components: { Posts, Loader },
  setup() {
    let limit = 10;
    let page = 1;
    let items = ref([]);
    let state = reactive({
      show: false,
      search: ""
    });
    async function getPosts() {
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/posts?_limit=${limit}&_page=${page}`
      );
      return res.json();
    }

    const filterItems = computed(()=>{
        if (state.search !== ''){
            return items.value.filter((item)=>{
                return (item.title.toLowerCase().indexOf(state.search.toLowerCase()) > -1 ||
                    item.body.toLowerCase().indexOf(state.search.toLowerCase()) > -1)
            })
        }
        return items.value;

    })
    function showLoading() {
      state.show = true;
      setTimeout(() => {
        state.show = false;
        setTimeout(async () => {
          page++;
          items.value = [...items.value, ...(await getPosts())];
          console.log(items);
        }, 300);
      }, 1000);
    }

    onMounted(async () => {
      items.value = await getPosts();

      window.addEventListener("scroll", () => {
        const {
          scrollTop,
          scrollHeight,
          clientHeight
        } = document.documentElement;

        let scrollTotal = scrollTop + clientHeight;
        if (scrollTotal >= scrollHeight) {
          showLoading();
        }
      });
    });

    return {
      items,
      state,
        filterItems
    };
  }
};
</script>

<style lang="scss"></style>
