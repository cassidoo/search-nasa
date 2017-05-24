<template>
  <div class="search">
    <form v-on:submit.prevent="getResults(query)">
      <input type="text" placeholder="query" v-model="query" />
    </form>
    <div class="results" v-if="results">
      <div v-for="result in results">
        <img v-bind:src="result.links[0].href" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'search',
  data () {
    return {
      query: '',
      results: ''
    }
  },
  methods: {
    getResults (q) {
      q = q.replace(/ /g, '%20')
      console.log(q)
      this.$http.get('https://images-api.nasa.gov/search?q=' + q + '&media_type=image').then(response => {
        this.results = response.body.collection.items
      }, response => {
        console.log('Error: ', response)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.search {
  margin: 20px auto;
}

.results img {
  height: 300px;
  margin: 10px;
}
</style>
