<template>
  <v-main class="dark lighten-3 pa-0 rounded">
    <v-container class="px-5">
      <v-row justify="center" align="center">
        <v-col cols="12" sm="8" md="6">
          <v-text-field
            v-model="search"
            :label="label"
            clearable
            append-icon="mdi-magnify"
          ></v-text-field>
        </v-col>
      </v-row>
      
      <v-row
        v-for="(item, index) in filteredCharacters"
        v-show="(page - 1) * per_page <= index && page * per_page > index"
        :key="item.id"
        justify="center"
        align="center"
      >
        <v-col cols="4" sm="2" md="2">
          <v-avatar color="orange" size="32">
            <span class="white--text text-h5">{{ item.id }}</span>
          </v-avatar>
        </v-col>
        <v-col cols="8" sm="6" md="6">
          {{ item.words.toString().replace(/,/g, ', ') }}
        </v-col>
      </v-row>
      <div v-if="search.length === 0" class="text-center">
        <v-pagination
          v-model="page"
          :length="Math.round(filteredCharacters.length / per_page)"
          circle
        ></v-pagination>
      </div>
    </v-container>
  </v-main>
</template>

<script>
import charades from '@/assets/json/charade.json'
export default {
  data: () => {
    return {
      label: 'Buscar',
      characters: [],
      search: '',
      page: 1,
      per_page: 10,
      accentsMap: {
        a: 'á|à|ã|â|À|Á|Ã|Â',
        e: 'é|è|ê|É|È|Ê',
        i: 'í|ì|î|Í|Ì|Î',
        o: 'ó|ò|ô|õ|Ó|Ò|Ô|Õ',
        u: 'ú|ù|û|ü|Ú|Ù|Û|Ü',
        c: 'ç|Ç',
        n: 'ñ|Ñ',
      },
    }
  },
  computed: {
    filteredCharacters() {
      if (!this.search) return this.characters

      return this.characters.filter((character) => {
        return character.words.find((field) => {
          const slugify = (text) =>
            Object.keys(this.accentsMap).reduce(
              (acc, cur) =>
                acc.replace(new RegExp(this.accentsMap[cur], 'g'), cur),
              text
            )
          field = slugify(field)
          return field.toLowerCase().includes(this.search.toLowerCase().trim())
        })
      })
    },
  },
  created() {
    this.fetchAllCharacters()
  },
  methods: {
    fetchAllCharacters() {
      this.characters = charades
    },
  },
}
</script>