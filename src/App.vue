<template>
    <v-app>
      <v-container fluid grid-list-xs>
          <v-layout class="blokJudul">
            <v-flex lg10 offset-lg1>
              <v-card color="">
                <v-card-title>
                  <v-icon>edit</v-icon>
                  <span class="judulLaporan">Laporan / Pendaftaran</span>
                </v-card-title>
              </v-card>
            </v-flex>
          </v-layout>
        <v-flex lg10 offset-lg1>
          <v-card color="" class="blokLaporan">
            <v-container fluid grid-list-xs>
              <v-layout row>
                <v-flex>
                  <dropdown></dropdown>
                </v-flex>
              </v-layout>
              <report></report>
            </v-container>
          </v-card>
        </v-flex>
      </v-container>
    </v-app>
</template>

<style scoped>

  .blokJudul {
    position: relative;
    top: 1em;
    /* width: 87.6em; */
  }

  .judulLaporan {
    position: relative;
    left: 0.5em;
  }

  .blokLaporan {
    position: relative;
    top: 2em;
  }
</style>

<script>
  import Report from '@/components/Report'
  import ReportMenu from '@/components/ReportMenu'
  import ReportSearcher from '@/components/ReportSearcher'

export default {
    methods: {
      onResize (event) {
        this.windowSize = { x: window.innerWidth, y: window.innerHeight }
      }
    },
    components: {
      'dropdown': ReportMenu,
      'searcher': ReportSearcher,
      'report': Report
    },
    name: 'App'
}
</script>
