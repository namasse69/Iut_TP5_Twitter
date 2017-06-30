<template>
  <div>
    <div>
          <strong> {{ tweet.auteur.prenom }} {{ tweet.auteur.nom }} </strong>
          <span class="handle">@{{ tweet.auteur.handle }}</span> - {{ moment(tweet.date).fromNow() }}
    </div>
    <div>
      {{ tweet.contenu }}
    </div>
    <div>
      <ul>
        <li class="button"><icon name="reply"/></li>
        <li class="button" v-if="isRetweetable()">
          <a @click="retweet()">
            <icon name="retweet"/>
            <span class="aside">{{ tweet.retweeters.length }}</span>
          </a>
        </li>
        <li class="button" v-else>
            <icon name="retweet"/>
            <span class="aside">{{ tweet.retweeters.length }}</span>
        </li>
        <li class="button"><icon name="heart"/></li>
        <li class="button"><icon name="envelope"/></li>
      </ul>
    </div>
  </div>
</template>

<script>
import 'vue-awesome/icons'
import Icon from 'vue-awesome/components/Icon'
import moment from 'moment'
export default {
  components: {Icon},
  name: 'tweet',
  props: ['tweet', 'connecUser'],
  methods: {
    moment: function (date) {
      return moment(date)
    },

    retweet: function () {
      this.$http.get('http://localhost:8080/retweet', {responseType: 'text', params: {utilisateur: this.connecUser, tweet: this.tweet.id}}).then(response => {
        this.$emit('retweeted', this.tweet.id)
      }, response => {})
    },
    isRetweetable: function () {
      console.log(this.connecUser)
      if (this.tweet.auteur.handle === this.connecUser) {
        return false
      }
      return true
    }
  },
  created () {
    moment.locale('fr')
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li.button {
 display: inline-block;
}

a {
 color: #42b983;
}

span.handle {
 color: gray;
}
</style>
