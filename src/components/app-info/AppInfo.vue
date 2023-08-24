<template>
  <div class="app-info">
    <p class="fs-3 text-uppercase">
      Barcha kinolar soni: {{ allMoviesCounter }}
    </p>
    <p class="fs-3 text-uppercase">
      Mashhur kinolar soni: {{ famousMoviesCounter }}
    </p>
    <p class="fs-3 text-uppercase">
      Ko'rilgan kinolar soni: {{ favouriteMoviesCounter }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    allMoviesCounter: {
      type: Number,
      required: true,
    },
    famousMoviesCounter: {
      type: Number,
      required: true,
    },
    favouriteMoviesCounter: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style scoped>
.app-info {
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
