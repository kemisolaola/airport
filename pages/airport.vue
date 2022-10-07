<template>
  <v-row justify="center" align="center" class="main-containers px-5">
    <v-col cols="12">
      <v-card rounded="lg" class="py-10">
        <v-row no-gutters class="px-4">
          <v-col cols="12" sm="6" md="3">
            <v-menu offset-y
            v-model="menuFrom"
             origin="center center"
             :close-on-content-click="false"
                      transition="scale-transition">
              <template v-slot:activator="{ on, attrs }">
                <v-card outlined height="120" class="pa-3" v-bind="attrs" v-on="on">
                  <p class="mb-1 card-subtitle">FROM</p>
                  <h2 class="card-h2">{{selectedFrom.city}}</h2>
                  <p class="mb-0 card-airport-name">{{selectedFrom.name}}</p>
                </v-card>
              </template>
              <v-app-bar height="40" class="px-0" color="white">
                <v-text-field prepend-inner-icon="mdi-magnify" v-model="searchTerm" solo flat background-color="inherit" dense hide-details height="40" placeholder="From"></v-text-field>
                </v-app-bar>
              <v-list width="400"
               height="400"
                class="overflow-y-auto">
                <v-list-item-content>
                  <v-list-item link color="white" v-for="(item, index) in filterByAirport" :key="index">
                    <v-row justify="space-between" @click="selectFrom(item)">
                      <v-col class="" cols="9">
                        <p class="mb-1 city-menu-list">{{item.city}}, {{item.country}}</p>
                        <p class="mb-4 airport-menu-list">{{item.name}} Airport</p>
                      </v-col>
                      <v-col class="text-right abr-menu-list"><span>{{item.IATA}}</span></v-col>
                    </v-row>
                  </v-list-item>
                </v-list-item-content>
              </v-list>
            </v-menu>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-menu offset-y v-model="menuTo"
            :close-on-content-click="false"
             origin="center center"
                      transition="scale-transition">
              <template v-slot:activator="{ on, attrs }">
                <v-card outlined height="120" class="pa-3" v-bind="attrs" v-on="on">
                  <p class="mb-1 card-subtitle">TO</p>
                  <h2 class="card-h2">{{selectedDestination.city}}</h2>
                  <p class="mb-0 card-airport-name">{{selectedDestination.name}}</p>
                </v-card>
              </template>
              <v-app-bar height="40" class="px-0" color="white">
                <v-text-field prepend-inner-icon="mdi-magnify" v-model="searchTerm" solo flat background-color="inherit" dense hide-details height="40"></v-text-field>
                </v-app-bar>
              <v-list width="400"
               height="400"
       class="overflow-y-auto">
                <v-list-item-content>
                  <v-list-item link color="white" v-for="(item, index) in filterByAirport" :key="index">
                    <v-row justify="space-between" @click="selectDestination(item)">
                      <v-col class="" cols="7">
                        <p class="mb-1 city-menu-list">{{item.city}}, {{item.country}}</p>
                        <p class="mb-4 airport-menu-list">{{item.name}}</p>
                      </v-col>
                      <v-col class="text-right"><small>{{item.IATA}}</small></v-col>
                    </v-row>
                  </v-list-item>
                </v-list-item-content>
              </v-list>
            </v-menu>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-menu
        ref="menu"
        v-model="menu"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="auto"
      ><template v-slot:activator="{ on, attrs }">
            <v-card outlined height="120"  class="pa-3" v-bind="attrs" v-on="on">
              <p class="mb-1 card-subtitle">DEPARTURE DATE & TIME<v-icon color="primary">mdi-chevron-down</v-icon></p>
              <p class="text-h5 font-weight-bold mb-0 card-h2" ><span class="card-h2">{{date | formattedDay}}</span><span  class="text-h6 font-weight-regular"> {{month[date | formattedMonth]}}' {{ date | formattedYear}}</span></p>
                  <p>{{days[(new Date(date)).getDay()]}}</p>
                </v-card>
                  </template>
                  <v-date-picker
          v-model="date"
          @input="menu  = false"
          scrollable
        >
        </v-date-picker>
      </v-menu>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-menu
        ref="menu"
        v-model="menu2"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="auto"
      ><template v-slot:activator="{ on, attrs }">
            <v-card outlined height="120"  class="pa-3" v-bind="attrs" v-on="on">
              <p class="mb-1 card-subtitle">RETURN DATE & TIME<v-icon color="primary">mdi-chevron-down</v-icon></p>
              <p class="text-h5 font-weight-bold  mb-0"><span class="card-h2">{{date2 | formattedDay}}</span><span  class="text-h6 font-weight-regular"> {{month[date2 | formattedMonth]}}' {{ date2 | formattedYear}}</span></p>
                  <span>{{days[(new Date(date2)).getDay()]}}</span>
                </v-card>
                  </template>
                  <v-date-picker
          v-model="date2"
          @input="menu2  = false"
          scrollable
        >
        </v-date-picker>
      </v-menu>
          </v-col>
        </v-row>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import airport from '~/db.js'
export default {
  name: 'IndexPage',
  data () {
    return {
      airports: [],
      selectedFrom: [],
      selectedDestination: [],
      date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      date2: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      days: ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
      month: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sept', 'Oct', 'Nov', 'Dec'],
      menu: false,
      menu2: false,
      searchTerm: '',
      menuTo: false,
      menuFrom: true
    }
  },
  filters: {
    formattedDay (val) {
      return (new Date(val)).getDate()
    },
    formattedMonth (val) {
      return [(new Date(val)).getMonth()]
    },
    formattedYear (val) {
      return (new Date(val)).getFullYear().toString().slice(-2)
    }
  },
  mounted () {
    // const res = await this.$axios.get('http://localhost:3000/countries')
    this.airports = airport
    this.selectedFrom = this.airports[0]
    this.selectedDestination = this.airports[5]
  },
  computed: {
    // Allows Search
    filterByAirport () {
      return this.airports.filter((city) => {
        return city.name.toLowerCase().includes((this.searchTerm).toLowerCase())
      })
    }
  },
  methods: {
    selectDestination (item) {
      this.selectedDestination = item
      this.menuTo = false
    },
    selectFrom (item) {
      this.selectedFrom = item
      this.menuFrom = false
    }
  }
}
</script>
