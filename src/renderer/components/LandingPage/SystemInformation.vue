<template>
  <div v-if="donateurs && donateurs.length">
    <div v-for="donateur of donateurs">
      <p><strong>{{donateur.prenom}} {{donateur.nom}}</strong></p>
      <p>{{donateur.montant}}</p>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  console.log('bob')
  export default {
    data () {
      return {
        donateurs: [],
        errors: []
      }
    },
    created: function () {
      this.fetchEventsList()
      this.timer = setInterval(this.fetchEventsList, 10000)
    },
    methods: {
      fetchEventsList: function () {
        console.log('Fetching from api...')
        axios({
          method: 'get',
          crossDomain: true,
          url: 'http://www.etienneloiselle.com/donateur/donateurs.json'
        }).then(response => {
          // JSON responses are automatically parsed.
          console.log(response.data)
          this.donateurs = response.data
        })
          .catch(e => {
            // this.errors.push(e)
          })
      },
      cancelAutoUpdate: function () { clearInterval(this.timer) }
    },
    beforeDestroy () {
      clearInterval(this.timer)
    }
}
</script>

<style scoped>
  .title {
    color: #888;
    font-size: 18px;
    font-weight: initial;
    letter-spacing: .25px;
    margin-top: 10px;
  }

  .items { margin-top: 8px; }

  .item {
    display: flex;
    margin-bottom: 6px;
  }

  .item .name {
    color: #6a6a6a;
    margin-right: 6px;
  }

  .item .value {
    color: #35495e;
    font-weight: bold;
  }
</style>