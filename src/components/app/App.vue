<template>
  <div class="container app font-monospace">
    <div class="contenet">
      <AppInfo
        :allMoviesCounter="movies.length"
        :famousMoviesCounter="movies.filter((c) => c.like).length"
        :favouriteMoviesCounter="movies.filter((c) => c.favourite).length"
      />
      <div class="search-panel">
        <SearchPanel :onUpdateTermHandler="onUpdateTermHandler" />
        <AppFilter
          :updateFilterHandler="updateFilterHandler"
          :filterName="filter"
        />
      </div>
      <div class="mt-4">
        <div
          class="box-div text-center fs-3 text-danger"
          v-if="!movies.length && !isLoading"
        >
          Kinolar yo'q
        </div>
        <div class="box-div" v-else-if="isLoading">
          <div class="spinner-border" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
        </div>
        <MovieList
          v-else
          :movies="onFilterHandler(onSearchHandler(movies, term), filter)"
          @onLike="onLikeHandler"
          @onFavourite="onFavouriteHandler"
          @onRemove="onRemoveHandler"
        />
        <div class="box-div">
          <nav
            aria-label="pagination"
            class="d-flex justify-content-center text-center"
          >
            <ul class="pagination pagination-sm">
              <li
                v-for="pageNumber in totalPages"
                :class="{ active: pageNumber === page }"
                :key="pageNumber"
                @click="changePageHandler(pageNumber)"
              >
                <span class="page-link">{{ pageNumber }}</span>
              </li>
            </ul>
          </nav>
        </div>
        <MovieAddFrom @createMovie="createMovie" />
      </div>
    </div>
  </div>
</template>

<script>
import AppInfo from "../app-info/AppInfo.vue";
import SearchPanel from "../search-panel/SearchPanel.vue";
import AppFilter from "../app-filter/AppFilter.vue";
import MovieList from "../movie-list/movielist.vue";
import MovieAddFrom from "../movie-add-from/MovieAddFrom.vue";
import axios from "axios";

export default {
  components: {
    AppInfo,
    SearchPanel,
    AppFilter,
    MovieList,
    MovieAddFrom,
  },

  data() {
    return {
      movies: [],
      term: "",
      filter: "all",
      isLoading: false,
      limit: 10,
      page: 1,
      totalPages: 0,
    };
  },

  methods: {
    async createMovie(item) {
      try {
        const response = await axios.post(
          "https://jsonplaceholder.typicode.com/posts",
          item
        );
        console.log(response);
        this.movies.push(response.data);
      } catch (error) {
        alert(error.message);
      }
    },
    onLikeHandler(id) {
      this.movies = this.movies.map((item) => {
        if (item.id == id) {
          item.like = !item.like;
        }
        return item;
      });
    },

    async onRemoveHandler(id) {
      try {
        const response = await axios.delete(`https://jsonplaceholder.typicode.com/posts/${id}`)
        console.log(response);
        this.movies = this.movies.filter((c) => c.id !== id);
      } catch (error) {
        alert(error.message);
      }
    },

    onFavouriteHandler(id) {
      this.movies = this.movies.map((item) => {
        if (item.id == id) {
          item.favourite = !item.favourite;
        }
        return item;
      });
    },

    onSearchHandler(arr, term) {
      if (term.length == 0) {
        return arr;
      }
      return arr.filter((c) => c.name.toLowerCase().indexOf(term) > -1);
    },

    onFilterHandler(arr, filter) {
      switch (filter) {
        case "popular":
          return arr.filter((c) => c.like);

        case "mostViewers":
          return arr.filter((c) => c.viewers > 9);
        default:
          return arr;
      }
    },

    onUpdateTermHandler(term) {
      this.term = term;
    },

    updateFilterHandler(filter) {
      this.filter = filter;
    },

    async fetchMovie() {
      try {
        this.isLoading = true;
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts",
          {
            params: {
              _limit: this.limit,
              _page: this.page,
            },
          }
        );
        const newArr = response.data.map((item) => ({
          id: item.id,
          name: item.title,
          like: false,
          favourite: false,
          viewers: item.id,
        }));
        this.totalPages = Math.ceil(
          response.headers["x-total-count"] / this.limit
        );
        this.movies = newArr;
        this.isLoading = false;
      } catch (error) {
        alert(error.message);
      }
    },
    changePageHandler(page) {
      this.page = page;
    },
  },
  mounted() {
    this.fetchMovie();
  },

  watch: {
    page() {
      this.fetchMovie();
    },
  },
};
</script>

<style>
.app {
  height: 100vh;
  color: #000;
}

.contenet {
  width: 1000px;
  min-height: 700px;
  background-color: white;
  margin: 0 auto;
  padding: 5rem 0;
}

.search-panel {
  margin-top: 2rem;
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}

.box-div {
  margin-top: 1.5rem;
  padding: 1.5rem;
  border-radius: 4px;
  background-color: #fcfaf5;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
