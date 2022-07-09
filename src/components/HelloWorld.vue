<template>
  <div id="app">
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else>
      <h2>Artists:</h2>
      <li v-for="artist in artists" :key="artist.id">
        <h4>{{ artist.attributes.name }}</h4>
        <h6>Projects:</h6>
        <ul v-if="artist.attributes.projects.data.length">
          <li v-for="project in artist.attributes.projects.data" v-bind:key="project.id">
            <p>{{ project.attributes.name }}</p>
            <img v-if="project.attributes.cover.data.attributes.url" :src="'http://localhost:1337' + project.attributes.cover.data.attributes.url" />
          </li>
        </ul>
        <span v-else>No projects</span>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Artists',
  data () {
    return {
      artists: [],
      error: null
    }
  },
  async mounted () {
    try {
      const { data } = await axios.get(`http://localhost:1337/api/artists?populate[projects][populate]=*`)
      this.artists = data.data;
      console.log(data.data);
    } catch (error) {
      this.error = error;
    }
  }
}
</script>

<style scoped>
ul {
  text-align: left;
}

img {
  max-width: 200px;
}
</style>