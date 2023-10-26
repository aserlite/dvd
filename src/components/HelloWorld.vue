import json from './json/data.json',
<template>
  <div class="dvd-library">
    <h1>DVDthèque</h1>

    <div class="filters">
      <input v-model="titleFilter" placeholder="Titre" />
      <input v-model="yearFilter" placeholder="Année" />
      <input v-model="languageFilter" placeholder="Langue" />
      <input v-model="actorFilter" placeholder="Acteur" />
    </div>

    <table class="dvd-table">
      <thead>
        <tr>
          <th>Titre</th>
          <th>Année</th>
          <th>Langue</th>
          <th>Acteurs</th>
          <th>Affiche</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="dvd in filteredDVDs" :key="dvd.id">
          <td v-green-on-hover>{{ dvd.title | capitalizeFirstAndLast }}</td>
          <td>{{ dvd.year }}</td>
          <td>{{ dvd.language }}</td>
          <td>{{ dvd.actors.join(', ') }}</td>
          <td> <router-link :to="{path:'/affiche/'+dvd.id}">Ouga</router-link></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      titleFilter: '',
      yearFilter: '',
      languageFilter: '',
      actorFilter: '',
      DVDs: require('../assets/data.json')
    }
  },
  computed: {
    filteredDVDs () {
      let filtered = this.DVDs

      if (this.titleFilter) {
        filtered = filtered.filter(dvd =>
          dvd.title.toLowerCase().includes(this.titleFilter.toLowerCase())
        )
      }

      if (this.yearFilter) {
        filtered = filtered.filter(dvd =>
          dvd.year.toString().includes(this.yearFilter)
        )
      }

      if (this.languageFilter) {
        filtered = filtered.filter(dvd =>
          dvd.language.toLowerCase().includes(this.languageFilter.toLowerCase())
        )
      }

      if (this.actorFilter) {
        filtered = filtered.filter(dvd =>
          dvd.actors.some(actor =>
            actor.toLowerCase().includes(this.actorFilter.toLowerCase())
          )
        )
      }

      return filtered
    }
  },
  filters: {
    capitalizeFirstAndLast (value) {
      if (!value) return ''
      return (
        value.charAt(0).toUpperCase() +
        value.slice(1, -1) +
        value.slice(-1).toUpperCase()
      )
    }
  },
  directives: {
    greenOnHover: {
      bind (elt) {
        elt.style.transition = 'color 0.3s'
        elt.addEventListener('mouseenter', () => {
          elt.style.color = 'green'
        })
        elt.addEventListener('mouseleave', () => {
          elt.style.color = ''
        })
      }
    }
  }
}
</script>

<style scoped>
.dvd-library {
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.dvd-table {
  width: 80%;
  border-collapse: collapse;
  margin-top: 20px;
}

.dvd-table th,
.dvd-table td {
  border: 1px solid #ccc;
  padding: 10px;
}

.dvd-table th {
  background-color: #f2f2f2;
}
</style>
