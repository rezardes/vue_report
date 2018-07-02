/* Data tabel harus ada */
/* Manajemen pengaturan properti tabel */

<template>
    <div>
      <v-layout row>
        <v-flex lg3 md3 sm4 xs7 offset-lg9 offset-md9 offset-sm7 offset-xs2>
          <div class="antarbaris">
            <v-text-field
              v-model="search"
              color="teal"
              label="Enter Keywords Here"
              single-line
              hide-details
            ></v-text-field>
          </div>
        </v-flex>
        <v-flex lg1>
          <v-btn small fab dark color="teal" @click="reset">
            <v-icon dark>close</v-icon>
          </v-btn>
        </v-flex>
      </v-layout>
      <v-flex lg12>
        <v-data-table
          :headers="headers"
          :search="search"
          :items="reports"
          class="elevation-1 tabel"
          item-key="nik"
        >
          <template slot="items" slot-scope="props">
            <tr>
              <td class="text-xs" style="text-align: center;"><span :class="iconStyle"><v-btn icon @click="props.expanded = !props.expanded, expander(props)">
                <v-icon color="teal"> {{ expandIcon[props.item.nik] }}</v-icon>
              </v-btn></span>{{ props.item.nik }}</td>
              <td class="text-xs" v-show="visibility.nama">{{ props.item.nama }}</td>
              <td class="text-xs" v-show="visibility.sid">{{ props.item.sid }}</td>
              <td class="text-xs" v-show="visibility.sre">{{ props.item.sre }}</td>
              <td class="text-xs" v-show="visibility.rdn">{{ props.item.rdn }}</td>
              <td class="text-xs" v-show="visibility.tanggal">{{ props.item.tanggal }}</td>
              <td class="text-xs" v-show="visibility.aliansi">{{ props.item.aliansi }}</td>
              <td class="text-xs" v-show="visibility.rdnbank">{{ props.item.rdnbank }}</td>
              <td class="justify-center layout px-0">
              </td>
            </tr>
          </template>
          <v-alert slot="no-results" class="zeroValidation" :value="true" color="error">
            <v-icon>warning</v-icon>
            <span class="validationText">"{{ search }}" tidak ditemukan!</span>
          </v-alert>
          <template slot="expand" slot-scope="props">
            <v-card flat>
              <v-card-text v-html="getExpandedReport(props.index) "></v-card-text>
            </v-card>
          </template>
        </v-data-table>
      </v-flex>
      <p> {{ windowSize }} </p>
    </div>
</template>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .antarbaris {
    margin-bottom: 10px;
  }

  .zeroValidation {
    text-align: center;
  }

  .validationText {
    position: relative;
    left: 0.5em;
  }

  .onBiggerIcon {
    margin-bottom: 0px;
    display: inline;
  }

  .onSmallerIcon {
    margin-bottom: 0px;
    display: block;
  }

</style>

<script>
  import Report from '@/components/Report'

  function zeroer (degree) {
    return ('0' + degree).slice(-2)
  }

  function formatDate (date) {
    return date.getFullYear() + '-' + zeroer(date.getMonth() + 1) + '-' + zeroer(date.getDate()) + ' ' + zeroer(date.getHours()) + ':' + zeroer(date.getMinutes()) + ':' + zeroer(date.getSeconds())
  }

  /* function getDate() {
    return new Date().getFullYear()+'-'+ zeroer(new Date().getMonth())+'-'+zeroer(new Date().getDate())+' '+new Date().getHours()+':'+new Date().getMinutes()+':'+new Date().getSeconds()
  } */

  export default {
    components: {
      'report': Report
    },
    data: () => ({
      iconStyle: '',
      search: '',
      expandIcon: {},
      expanded: {},
      isExpanded: false,
      headers: [
        {
          text: 'NIK',
          align: 'left',
          value: 'nik'
        },
        { text: 'Nama', align: 'left', value: 'nama' },
        { text: 'SID', align: 'left', value: 'sid' },
        { text: 'SRE', align: 'left', value: 'sre' },
        { text: 'RDN', align: 'left', value: 'rdn' },
        { text: 'Tanggal Input', align: 'left', value: 'tanggal' },
        { text: 'Aliansi', align: 'left', value: 'aliansi' },
        { text: 'RDN Bank', align: 'left', value: 'rdnbank' }
      ],
      expandedHeaders: [],
      visibility: {},
      headerKeys: [],
      hiddenHeaders: [],
      sizePoint: [
        {
          breakpoint: 1067,
          reached: false
        },
        {
          breakpoint: 947,
          reached: false
        },
        {
          breakpoint: 847,
          reached: false
        },
        {
          breakpoint: 684,
          reached: false
        },
        {
          breakpoint: 596,
          reached: false
        },
        {
          breakpoint: 511,
          reached: false
        }
      ],
      centerPoint: {
        breakpoint: 429,
        reached: false
      },
      reports: [],
      expandedReports: [],
      editedIndex: -1,
      windowSize: {
        x: 0,
        y: 0
      }
    }),

    created () {
      this.initialize()
      this.reports.forEach(item => {
        this.$set(this.expanded, item.nik, false)
        this.$set(this.expandIcon, item.nik, 'add_circle_outline')
      })
      this.makeCol()
      this.initVisibility()
    },

    mounted () {
      window.addEventListener('resize', this.onResize)
      this.onResize()
    },

    methods: {
      expander (props) {
        this.expanded[props.item.nik] = !this.expanded[props.item.nik]
        this.expandIcon[props.item.nik] = (this.expanded[props.item.nik]) ? 'add_circle' : 'add_circle_outline'
      },
      testing (par) {
        return 'Tes ' + par.nama
      },
      getDate () {
        return new Date().getFullYear()
      },
      reset () {
        this.search = ''
      },
      initVisibility () {
        this.headerKeys.forEach((item) => {
          this.$set(this.visibility, item, true)
        })
      },
      initialize () {
        this.reports = [
          {
            nik: '5271030110690001',
            nama: 'DANNY ZALDY RAPPUNG',
            sid: '',
            sre: '',
            rdn: '',
            tanggal: formatDate(new Date('2017-10-03 09:44:06')),
            aliansi: 'TRIM',
            rdnbank: 'BBCA'
          },
          {
            nik: '7174041612850002',
            nama: 'ANDI ABDUL WARIS',
            sid: '',
            sre: '',
            rdn: '',
            tanggal: formatDate(new Date('2017-09-18 10:43:54')),
            aliansi: 'TRIM',
            rdnbank: 'BBCA'
          },
          {
            nik: '7371035810830009',
            nama: 'SILVYA LUSMAN',
            sid: '',
            sre: '',
            rdn: '',
            tanggal: formatDate(new Date('2017-11-09 10:02:19')),
            aliansi: 'TRIM',
            rdnbank: 'BBCA'
          },
          {
            nik: '7371071909840004',
            nama: 'SULIHIN LIEM JAYA',
            sid: '',
            sre: '',
            rdn: '',
            tanggal: formatDate(new Date('2017-10-23 10:28:10')),
            aliansi: 'TRIM',
            rdnbank: 'BBCA'
          },
          {
            nik: '7371096003750007',
            nama: 'CLAUDIA KEATHING CHANG',
            sid: '',
            sre: '',
            rdn: '',
            tanggal: formatDate(new Date('2017-09-13 11:14:35')),
            aliansi: 'TRIM',
            rdnbank: 'BBCA'
          },
          {
            nik: '7306021308920002',
            nama: 'SYAMSUL ALAM',
            sid: '',
            sre: '',
            rdn: '',
            tanggal: formatDate(new Date('2017-11-08 11:17:51')),
            aliansi: 'TRIM',
            rdnbank: 'NRDN'
          }
        ]

        this.expandedHeaders = [
          'CIF', 'DOB', 'Tipe FPR', 'Di Input Oleh', 'NPWP'
        ]

        this.expandedReports = [
          {
            cif: '',
            dob: '18-10-1983',
            tipeFPR: 'Saham',
            diInputOleh: 'isye.marlin@trimegah.com',
            npwp: ''
          },
          {
            cif: '',
            dob: '13-08-1992',
            tipeFPR: 'Saham',
            diInputOleh: 'isye.marlin@trimegah.com',
            npwp: ''
          },
          {
            cif: '',
            dob: '19-09-1984',
            tipeFPR: 'Saham',
            diInputOleh: 'isye.marlin@trimegah.com',
            npwp: '442397089801000'
          },
          {
            cif: '',
            dob: '01-10-1969',
            tipeFPR: 'Saham',
            diInputOleh: 'isye.marlin@trimegah.com',
            npwp: '148693260821000'
          },
          {
            cif: '',
            dob: '16-12-1985',
            tipeFPR: 'Saham',
            diInputOleh: 'isye.marlin@trimegah.com',
            npwp: '141053678821000'
          },
          {
            cif: '',
            dob: '20-03-1975',
            tipeFPR: 'Saham',
            diInputOleh: 'isye.marlin@trimegah.com',
            npwp: ''
          }
        ]
      },
      getExpandedReport (index) {
        var ltr = ''
        var list = Object.keys(this.expandedReports[index])
        ltr = ltr + '<table style="text-align: left;">'
        list.forEach((item, idx) => {
          ltr = ltr + '<tr><th>' + this.expandedHeaders[idx] + '</th><th style="font-weight: normal;">' + this.expandedReports[index][list[idx]] + '</th></tr>'
        })
        ltr = ltr + '</table>'
        return ltr
      },
      makeCol () {
        this.headerKeys = Object.keys(this.reports[0])
      },
      onResize (event) {
        this.windowSize = { x: window.innerWidth, y: window.innerHeight }

        this.sizePoint.forEach((item, index) => {
          if (window.innerWidth < item.breakpoint && !item.reached) {
            this.onSmaller(index)
          }

          if (window.innerWidth > item.breakpoint && item.reached) {
            this.onBigger(index)
          }
        })
        this.setIconStyle()
      },
      setIconStyle () {
        if (!this.centerPoint.reached && window.innerWidth < this.centerPoint.breakpoint) {
          this.centerPoint.reached = true
          this.iconStyle = 'onSmallerIcon'
        } else if (this.centerPoint.reached && window.innerWidth >= this.centerPoint.breakpoint)  {
          this.centerPoint.reached = false
          this.iconStyle = 'onBiggerIcon'
        }
      },
      onSmaller (index) {
        var lastHeader = this.headers[this.headers.length - 1].value
        this.sizePoint[index].reached = true
        this.hiddenHeaders.push(this.headers[this.headers.length - 1])
        this.visibility[lastHeader] = false
        this.expandedHeaders.splice(this.expandedHeaders.length, 0, this.headers[this.headers.length - 1].text)
        this.expandedReports.forEach((item, index) => {
          this.$set(item, lastHeader, this.reports[index][lastHeader])
        })
        this.headers.splice(this.headers.length - 1, 1)
        console.log(this.expandedHeaders)
      },
      onBigger (index) {
        this.sizePoint[index].reached = false
        this.headers.splice(this.headers.length, 0, this.hiddenHeaders[this.hiddenHeaders.length - 1])
        this.hiddenHeaders.pop()
        this.visibility[this.headers[this.headers.length - 1].value] = true
        this.expandedHeaders.splice(this.expandedHeaders.length - 1, 1)
        this.expandedReports.forEach((item, index) => {
          Reflect.deleteProperty(item, this.headers[this.headers.length - 1].value)
        })
        console.log(this.expandedHeaders)
      }
    },
    name: 'App'
  }
</script>
