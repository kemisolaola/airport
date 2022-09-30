<template>
  <v-row justify="center" align="center" class="main-containers px-5">
    <v-col cols="12">
      <v-card rounded="lg" height="300" class="py-16">
        <v-row no-gutters class="px-4">
          <v-col cols="12" sm="6" md="3">
            <v-menu offset-y
             origin="center center"
                      transition="scale-transition">
              <template v-slot:activator="{ on, attrs }">
                <v-card outlined height="150" class="pa-3" v-bind="attrs" v-on="on">
                  <p class="mb-1">FROM</p>
                  <h2>{{selectedFrom.city}}</h2>
                  <p class="mb-0">{{selectedFrom.name}}</p>
                </v-card>
              </template>
              <v-list width="400"
               height="400"
       class="overflow-y-auto">
                <v-list-item-content>
                  <v-list-item link color="white" v-for="(item, index) in airports" :key="index">
                    <v-row justify="space-between" @click="selectedFrom = item">
                      <v-col class="">
                        <p class="mb-1 city-list">{{item.city}}, {{item.country}}</p>
                        <p class="mb-4 city-list">{{item.name}}</p>
                      </v-col>
                      <v-col class="text-right"><small>{{item.IATA}}</small></v-col>
                    </v-row>
                  </v-list-item>
                </v-list-item-content>
              </v-list>
            </v-menu>
          </v-col>
          <v-col cols="12" sm="6" md="3">
            <v-menu offset-y
             origin="center center"
                      transition="scale-transition">
              <template v-slot:activator="{ on, attrs }">
                <v-card outlined height="150" class="pa-3" v-bind="attrs" v-on="on">
                  <p class="mb-1">TO</p>
                  <h2>{{selectedDestination.city}}</h2>
                  <p class="mb-0">{{selectedDestination.name}}</p>
                </v-card>
              </template>
              <v-list width="400"
               height="400"
       class="overflow-y-auto">
                <v-list-item-content>
                  <v-list-item link color="white" v-for="(item, index) in airports" :key="index">
                    <v-row justify="space-between" @click="selectedDestination = item">
                      <v-col class="">
                        <p class="mb-1 city-list">{{item.city}}, {{item.country}}</p>
                        <p class="mb-4 city-list">{{item.name}}</p>
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
            <v-card outlined height="150"  class="pa-3" v-bind="attrs" v-on="on">
              <p class="mb-1">DEPARTURE<v-icon color="primary">mdi-chevron-down</v-icon></p>
              <p class="text-h5 font-weight-bold" >{{date | formattedDay}}<span  class="text-h6 font-weight-regular"> {{month[date | formattedMonth]}}' {{ date | formattedYear}}</span></p>
                  <p>{{days[(new Date(date)).getDay()]}}</p>
                </v-card>
                  </template>
                  <v-date-picker
          v-model="date"
          @input="menu  = false"
          no-title
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
            <v-card outlined height="150"  class="pa-3" v-bind="attrs" v-on="on">
              <p class="mb-1">RETURN<v-icon color="primary">mdi-chevron-down</v-icon></p>
              <p class="text-h5 font-weight-bold" >{{date2 | formattedDay}}<span  class="text-h6 font-weight-regular"> {{month[date2 | formattedMonth]}}' {{ date2 | formattedYear}}</span></p>
                  <span>{{days[(new Date(date2)).getDay()]}}</span>
                </v-card>
                  </template>
                  <v-date-picker
          v-model="date2"
          @input="menu2  = false"
          no-title
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
      menu2: false
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
  async mounted () {
    const res = await this.$axios.get('http://localhost:3000/countries')
    this.airports = res.data
    console.log(this.airports)
    this.selectedFrom = this.airports[0]
    this.selectedDestination = this.airports[5]
  }
}
</script>
<style scoped>
.main-containers {
  background-image: linear-gradient(to bottom, #051322, #15457c);
}

.city-list {
  font-size: 14px;
}
</style>
