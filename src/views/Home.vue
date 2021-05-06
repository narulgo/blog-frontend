<template>
  <div class="home">
    <div v-for="article in articles" :key="article.pk">
      {{ article.author }}
    </div>
  </div>
</template>

<script>
import {csrftoken} from '../csrf/csrf_token'
export default {
  name: 'Home',
  data() {
    return {
      articles: []
    }
  },
  async created () {
    const response = await fetch('http://127.0.0.1:8000/api/articles/')
    this.articles = await response.json()
  },
  methods: {
    getAllArticles() {
      fetch('http://localhost:8000/api/articles/', {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          'X-CSRFTOKEN': csrftoken
        }
      })
      .then(resp=>resp.json())
      .then((data) => {
        this.articles.push(...data)
      })
      .catch(error => console.log(JSON.stringify(error)))
    }
  }
}
</script>
