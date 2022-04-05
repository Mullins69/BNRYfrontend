<template>
  <div class="container" v-if="TopHeadLines">
    <div class="row">
      <h2>Top Technology Headlines Today</h2>
    </div>
    <div class="row">
      <div class="col-4">
        <select
          v-model="selected"
          class="form-select"
          aria-label="Default select example"
        >
          <option selected value="">Display All</option>
          <option value="mask">Masks</option>
          <option value="wetsuits">Wetsuits</option>
          <option value="camera">Camera</option>
        </select>
      </div>
      <div class="col-4">
        <form class="d-flex" @submit.prevent="querySearch">
          <input
            class="form-control me-2"
            type="text"
            v-model="query"
            placeholder="Search"
            aria-label="Search"
          />
          <button type="submit">
            search
          </button>
        </form>
      </div>
      <div class="col-4"></div>
    </div>
    <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
      <div class="col" v-for="article of TopHeadLines.articles" :key="article">
        <div class="card">
          <div class="card-image-wrapper">
            <img
              :src="article.urlToImage"
              class="card-img-top img-fluid"
              alt="NewsApi Pic Unavailable"
            />
          </div>
          <div class="card-body">
            <h5 class="card-title">{{ article.title }}</h5>
            <p class="card-text">
              {{ article.description }}
            </p>
            <a :href="article.url" target="_blank" class="btn btn-primary"
              >ReadMore</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
export default {
  name: "HomeView",
  data() {
    return {
      TopHeadLines: [],
      query: "",
    };
  },
  components: {
    HelloWorld,
  },
  methods: {
    querySearch() {
      fetch("http://localhost:3000/", {
        method: "GET",
        body: JSON.stringify({
          q: this.query,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.TopHeadLines = json;
        });
    },
  },
  created() {
    fetch("http://localhost:3000/")
      .then((response) => response.json())
      .then((json) => {
        this.TopHeadLines = json;
        console.log(this.TopHeadLines);
      });
  },
};
</script>
