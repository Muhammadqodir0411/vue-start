 <template>
  <div>
    <input
      class="form-control search-input"
      type="text"
      placeholder="Kinolarni qidirish"
      :value="term"
      @input="changeHandler"
    />
  </div>
</template>

<script>
export default {
  props: {
    onUpdateTermHandler: {
      type: Function,
      required: true,
    },
  },

  data() {
    return {
      term: '',
    }
  },

  methods: {
    changeHandler(e) {
      this.term = e.target.value
      this.onUpdateTermHandler(this.term)
    }
  }
};
</script>

<style scoped>
.search-input {
  margin-top: 1rem;
  margin-bottom: 1rem;
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
