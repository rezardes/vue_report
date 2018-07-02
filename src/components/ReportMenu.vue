<template>
  <v-menu open-on-hover bottom offset-y>
    <v-btn slot="activator" color="teal" dark>
      M E N U
      <v-icon>
        arrow_drop_down
      </v-icon>
    </v-btn>
    <v-list dense color="teal">
      <v-list-tile v-for="(mainItem, mainIndex) in mainItems" :key="mainIndex" @click="">
        <div class="expandMenu" v-if="mainIndex!=2">
          <v-menu open-on-hover right offset-x>
            <v-list-tile slot="activator" @click="">
              <v-list-tile-title> {{ mainItem.title }} </v-list-tile-title>
              <v-list-tile-action :class="arrowIcon(mainIndex)">
                <v-icon>arrow_right</v-icon>
              </v-list-tile-action>
            </v-list-tile>
            <v-list dense>
              <v-list-tile v-for="(subMenuItem, subMenuIndex) in subMenuItems[mainIndex]" :key="subMenuIndex" @click="">
                <v-list-tile-title> {{ subMenuItem.title }} </v-list-tile-title>
              </v-list-tile>
            </v-list>
          </v-menu>
        </div>
        <div class="nonExpandMenu" v-else>
          <v-list-tile>
            <v-list-tile-title> {{ mainItem.title }} </v-list-tile-title>
          </v-list-tile>
        </div>
      </v-list-tile>
    </v-list>
  </v-menu>
</template>

<script>
  export default {
    data: () => ({
      mainItems: [
        { title: 'Generate PDF' },
        { title: 'Export' },
        { title: 'Upload File Pendukung' }
      ],
      subMenuItems: [
        [
          { title: 'RDN' },
          { title: 'FPR Full' },
          { title: 'FPR Sign Page Only' },
          { title: 'FPR ESA' },
          { title: 'Ket. Tanpa NPWP' }
        ],
        [
          { title: 'Copy' },
          { title: 'Excel' },
          { title: 'CSV' }
        ],
        []
      ]
    }),
    methods: {
      arrowIcon: function (count) {
        return 'arrow-icon' + count + ' justify-end'
      }
    },
    name: 'ReportMenu'
  }
</script>

<style scoped>
  .arrow-icon0 {
    position: relative;
    left: 1.6em;
  }
  .arrow-icon1 {
    position: relative;
    left: 5em;
  }
</style>
