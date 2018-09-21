<template>
  <div id="app">
    <div class="stats-container">
      <div :key="index" v-for="(user, index) in orderedUsers" class="user-stats" v-if="user.userData">
        <div class="user-stats__title">
          <span class="user-stats__username">{{user.username}}</span>
        </div>
        <div class="user-stats__container">
          <div class="user-stats__block">
            <span>Kills: {{user.userData.stats.kills}}</span>
            <span>Deaths: {{user.userData.stats.deaths}}</span>
            <span>Kd: {{parseFloat(user.userData.stats.kills/user.userData.stats.deaths).toFixed(5)}}</span>
            <span>Score: {{user.userData.player.score}}</span>
          </div>
          <div class="user-stats__block">
            <img :src="getUserRankImage(user)" alt="">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import {orderBy} from 'lodash'
export default {
  name: 'App',

  mounted(){
    this.loadAllUsers(this.users);
    console.log(orderBy)
  },
  methods: {
    loadAllUsers(users) {
      let vm = this;
      users.map((user, index) => {
        vm.getUserData(index);
      })
    },

    getUserData(index) {
      let vm = this;
      fetch('https://api.bf4stats.com/api/playerInfo?plat=pc&opt=stats&name='+vm.users[index].username, {cache:'no-store'})
        .then(res => {
          return res.json().then(object => {
            object.stats.kills = object.stats.kills - vm.users[index].startKills
            object.stats.deaths = object.stats.deaths - vm.users[index].startDeaths
            object.player.score = object.player.score - vm.users[index].startScore
            vm.$set(vm.users[index], 'userData', object)
          })
        })
    },

    getUserRankImage(user) {
      return require('./assets/bf4/ranks/r'+user.userData.player.rank.nr+'.png')
    }
  },
  data() {
    return {
      users: [
        {
          username: 'asalways66',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        },
        {
          username: 'P3ter97',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        },
        {
          username: '3ndargon',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        },
        {
          username: 'arthur14725',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        },
        {
          username: 'displee',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        },
        {
          username: 'NickleBackFan420',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        },
        {
          username: 'bedela1',
          startKills: 0,
          startDeaths: 0,
          startScore: 0,
        }
      ]
    }
  },
  computed: {
      orderedUsers: function() {
        return orderBy(this.users, 'userData.player.score', 'desc')
      }

  }
}
</script>

<style lang="scss">

* {
  font-family: 'Russo One', sans-serif;
}
html {
  background: url('https://media.contentapi.ea.com/content/dam/bf/en-us/migrated-images/2017/04/bf4-levolution1.jpg.adapt.crop16x9.1920w.jpg');
}

.stats-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.user-stats {
  background-image: url('https://upload.wikimedia.org/wikipedia/commons/c/c6/M81_U.S._woodland_camouflage_pattern_swatch.png');
  margin: 10px;
  padding: 5px;
  display: grid;
  border-radius: 8px;
  .user-stats__title {
    text-align: center;
    color: green;
  }
  .user-stats__container {
    position: relative;
    span {
      display: block;
      margin-top:10px;
    }
    img {
      position: absolute;
      top: 10%;
      left: 80%;
      max-width: 100px;
    }
  }

}
</style>
