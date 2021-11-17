<template>
  <v-card class="mx-auto" max-width="850">
    <v-toolbar color="blue-grey" dark>
      <v-spacer></v-spacer>
      <v-toolbar-title>Listadado</v-toolbar-title>
    </v-toolbar>

    <v-container fluid>
      <v-row dense>
        <v-col v-for="card in items" :key="card.id" :cols="dflex">
          <v-card height="120">
            <v-card-title v-text="card.id"></v-card-title>
            <v-card-subtitle 
              >{{ card.words.toString().replace(/,/g, ', ') }}</v-card-subtitle
            >
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>
<script>
import charades from '@/assets/json/charade.json'
export default {
  data: () => ({
    dflex: 3,
    items : charades,
    
  }),
}
</script>d