<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Get Metadati Radio Steaming</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto" prepend-icon="mdi-music-box-multiple-outline" subtitle="" width="400">
          <template v-slot:title>
            <audio ref="audioPlayer" :src="audioUrl" @loadedmetadata="fetchMetadata" controls></audio>
            <!-- <span class="font-weight-black">Welcome to Vuetify</span> -->
          </template>

          <v-card-text class="bg-surface-light pt-4">
            {{ this.metadata }}
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn @click="this.splitMetadata()">Estrai</v-btn>
      </v-col>
      <v-col>
        <v-card>
          <v-card-title class="mb-2">
            <span class="font-weight-black">Informazioni</span>
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-text-field label="Titolo" v-model="meta.titolo" readonly></v-text-field>
            </v-row>
            <v-row>
              <v-text-field label="Artista" v-model="meta.artista" readonly></v-text-field>
            </v-row>
            <v-row>
              <v-text-field label="Genere" v-model="meta.x" readonly></v-text-field>
            </v-row>
            <v-row>
              <v-text-field label="Anno" v-model="meta.anno" readonly></v-text-field>
            </v-row>
            <v-row>
              <v-text-field label="Radio" v-model="meta.radio" readonly></v-text-field>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

  </v-container>
</template>

<script>
import { createMetadataClient } from '@radiolise/metadata-client'


export default {
  data() {
    return {
      audioUrl: "http://icy.unitedradio.it/105Dance90.mp3",
      metadata: null,
      meta: {
        titolo: '',
        artista: '',
        x: '',
        anno: '',
        radio: '',
      }
    };
  },
  methods: {
    fetchMetadata() {
      const nowPlaying = createMetadataClient({
        // Official instance URL
        url: 'wss://backend.radiolise.com/api/data-service',
      })

      // Register subscription handler
      // eslint-disable-next-line
      const subscription = nowPlaying.subscribe(({ title, error }) => {
        if (!error) {
          this.metadata = title
        }
      })
      nowPlaying.trackStream('http://icy.unitedradio.it/105Dance90.mp3')
    },
    splitMetadata() {
      const parti = this.metadata.split('~');
      this.meta.titolo = parti[0];
      this.meta.artista = parti[1];
      this.meta.x = parti[2];
      this.meta.anno = parti[3];
      this.meta.radio = parti[8];
      console.log(parti); 
    }
  },
  created() {
    this.fetchMetadata();
  }
};
</script>
