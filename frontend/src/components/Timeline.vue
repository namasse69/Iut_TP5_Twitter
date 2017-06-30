<template>
  <div>
    <utilisateur :utilisateurs="utilisateurs" @connecUser="connecUser"></utilisateur>
    <feed :tweets="tweets" :connecUser="connecUser" @retweeted="retweet" :loading="loading" ></feed>
  </div>
</template>

<script>
import Feed from './Feed'
import Utilisateur from './Utilisateur'
import Vue from 'vue'
import Resource from 'vue-resource'
Vue.use(Resource)

export default {
  name: 'hello',
  components: {Feed, Utilisateur},
  created () {
    this.fetchTweets()
    this.fetchUsers()
  },
  methods: {
    fetchTweets: function () {
      this.$http.get('http://localhost:8080/list').then(response => {
        this.tweets = response.body
        this.loading = false
      }, response => {
      })
    },
    fetchUsers: function () {
      this.$http.get('http://localhost:8080/list').then(response => {
        this.utilisateurs = response.body
        this.loading = false
      }, response => {
      })
    },
    retweet: function (id) {
      var tweet = this.tweets.find(tweet => id === tweet.id)
      tweet.retweeters.push({handle: 'johndoe'})
    },
    connecUser: function (user) {
      this.connecUser = user
    }
  },
  data () {
    return {
      connecUser: null,
      tweets: [],
      utilisateurs: [],
      loading: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 100px;
}

a {
  color: #42b983;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 0 10px;
}
</style>
