<template>
  <div>
    <h2>Homepage</h2>
      <item v-for="story in stories" :key="story.data.id" :story="story"/>
  </div>
  
</template>

<script>
import axios from 'axios'
import Item from '@/components/Item'

export default {
  name: 'homepage',
  components: {
    'item': Item
  },
  data: function () {
    return {
      err: '',
      stories: []
    }
  },
  created: function () {
    // https://hacker-news.firebaseio.com/v0/topstories.json
    axios.get('https://hacker-news.firebaseio.com/v0/newstories.json')
      // .then((result) => { this.stories = result.data })
      .then((response) => {
        let results = response.data.slice(0, 10)
          results.forEach(id => {
            axios.get('https://hacker-news.firebaseio.com/v0/item/' + id + '.json')
              .then((response) => {
                this.stories.push(response)
              })
              .catch((err) => {
                this.err = err
              })
          })
      })
      .catch((err) => { this.err = err 
    }) 
  }
}
  
</script>

<style scoped>

</style>
