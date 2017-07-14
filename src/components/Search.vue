<template>
  <div class="container">
    <div class="columns">
      <div class="column is-half is-offset-one-quarter">
        <h1 class="title">NASA Image Search</h1>
        <div class="field has-addons">
          <p class="control">
            <input class="input" @keydown.enter="getResults(query)" type="text" placeholder="Type your search here..." v-model="query" />
          </p>
          <p class="control">
            <a class="button is-primary" @click="getResults(query)">Search</a>
          </p>
          </form>
        </div>
        <div class="results" v-if="results.length">
          <div v-for="result in results" :key="result.data[0].nasa_id">
            <img :src="result.links[0].href" />
          </div>
        </div>
        <div v-else>
          <p>Sorry, nothing found for "
            <strong>{{ query }}</strong>".</p>
        </div>
      </div>
    </div>
  
  </div>
</template>

<script>
  import axios from 'axios'

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
        axios.get('https://images-api.nasa.gov/search?q=' + q + '&media_type=image')
          .then(response => {
            this.results = response.data.collection.items
          })
          .catch(function (error) {
            console.log(error);
          });
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .field.has-addons {
    justify-content: center;
  }
  
  .results {
    max-height: 70vh;
    overflow-y: auto;
    margin-top: 20px;
  }
 
</style>
