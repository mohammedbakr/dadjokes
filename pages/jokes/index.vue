<template>
  <div>
    <app-search-jokes @search-text="searchText" />
    Jokes
    <app-joke v-for="joke in jokes" :key="joke.id" :joke="joke.joke" :id="joke.id"></app-joke>
  </div>
</template>

<script>
import axios from 'axios';
import Joke from '@/components/Joke';
import SearchJokes from '@/components/SearchJokes';

export default {
  components: {
    AppJoke: Joke,
    AppSearchJokes: SearchJokes
  },

  data() {
    return {
      jokes: []
    }
  },

  async created() {
    const config = {
      headers: {
        'Accept': 'application/json'
      }
    };

    try {
      const res = await axios.get('https://icanhazdadjoke.com/search', config);
      
      this.jokes = res.data.results;
    } catch (error) {
      console.log(error);
    }
  },

  methods: {
    async searchText(text) {
      const config = {
      headers: {
        'Accept': 'application/json'
      }
    };

    try {
      const res = await axios.get(`https://icanhazdadjoke.com/search?term=${text}`, config);
      
      if (res.data.results.length > 0) {
        this.jokes = res.data.results;
      } else {
        alert(`No Jokes contain ${text}`);
      }
    } catch (error) {
      console.log(error);
    }
    }
  },

  head() {
    return  {
      title: 'Dad Jokes',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Best place for corny dad jokes'
        }
      ]
    }
  }
}
</script>

<style>

</style>