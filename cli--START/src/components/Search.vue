<template>
  <section class="search">
    <h1>Search by Github [[ selectedSearchMethod ]]</h1>
    <form @submit.prevent="searchRepos">
      <input type="search" name="search" id="search" v-model="q" required>
      <label for="search">[[ selectedSearchMethod ]] search</label>
      <!-- Add component to template -->
      <!-- Continue to pass down props -->
    </form>
  </section>
</template>

<script>
// import component

export default {
  name: 'Search',
  // components: { add component to component object },
  data() {
    return {
      q: '',
    };
  },
  props: {},
  methods: {
    async searchRepos() {
      this.$emit('handleSearch', {
        key: 'isSearching',
        val: true,
      });
      this.$emit('resetSearch');
      const response = await fetch(`https://api.github.com/search/repositories?q=${this.q}`);
      
      // There's a different endpoint if the search method changes
      // Developer endpoint: https://api.github.com/users/${this.q}/repos

      const json = await response.json();

      const items = json.items;
      // We will need to define object based on search type, since it returns a different data structure

      this.$emit('handleSearch', {
        key: 'isSearching',
        val: false,
      });

      if (items.length) {
        this.$emit('handleChange', {
          key: 'repos',
          val: items,
        });
      } else {
        this.$emit('handleSearch', {
          key: 'hasError',
          val: true,
        });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>

.search
  display: grid
  justify-content: center
  form
    position: relative
    label
      position: absolute
      left: 6px
      top: 10px
      color: var(--grey)
      background: white
      padding: 5px
      line-height: 1
      transition: 0.25s ease;
    input
      width: 100%
      box-sizing: border-box
      padding: 10px
      border-radius: 5px
      border: 2px solid var(--grey)
      &:hover,
      &:active,
      &:focus,
      &:valid
        &~ label
          top: -13px

</style>
