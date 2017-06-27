<template>
  <div>
    <p v-if="loading">Chargement des tweets en cours ...</p>
    <ul v-else>
      <li v-for="tweet in tweets">
        <tweet :tweet="tweet"></tweet>
      </li>
    </ul>
  </div>
</template>

<script>
import Tweet from './Tweet'

export default {
  name: 'feed',
  props: ['tweets', 'loading'],
  components: {Tweet}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
