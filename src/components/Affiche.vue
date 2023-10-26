<template>
    <div>
      <router-link to="/">Retour</router-link>
      <div>
        <h3>{{ dvd.title }}</h3>
        <h5>Acteurs : <br/>{{ dvd.actors.join(', ') }}</h5>
        <img :src="dvd.affiche">
      </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      id: this.$route.params.id,
      DVDs: require('../assets/data.json'),
      dvd: null
    }
  },
  created () {
    this.dvd = this.DVDs.find(dvd => dvd.id === parseInt(this.id, 10))
  }
}
</script>
