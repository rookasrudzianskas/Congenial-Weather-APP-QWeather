<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-px-md" >
      <q-input
        @keyup.enter="getWeatherBySearch"
        v-model="search"
        placeholder="Search"
        dark
        borderless
        >
        <template v-slot:before>
          <q-icon
            name="my_location"
            @click="getLocation"
          />
        </template>

        <template v-slot:append>
          <q-btn
            @click="getWeatherBySearch"
            round
            dense
            flat
            icon="search"
          />
        </template>
      </q-input>
    </div>
    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          {{ weatherData.name }}
        </div>
        <div class="text-h6 text-weight-light">
          {{weatherData.weather[0].main}}
        </div>

        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(weatherData.main.temp)}}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>
      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon }@2x.png`">
      </div>
    </template>
    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Weather
        </div>

        <q-btn class="col" flat @click="getLocation">
          <q-icon
            left
            size="3em"
            name="my_location"
            />
          <div>Find my location</div>
        </q-btn>

      </div>
    </template>
    <div class="col skyline">

    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null,
      lat: null,
      lon: null,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '7072edc8ba7df4103c3a2efc6061be00',
    }
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(position => {
        console.log(position)
        this.lat = position.coords.latitude
        this.lon = position.coords.longitude
        this.getWeatherByCoords()

      })
    },
    getWeatherByCoords() {
      this.$axios(`${ this.apiUrl }?lat=${this.lat}&lon=${this.lon}&appid=${ this.apiKey }&units=metric`).then(response => {
        this.weatherData = response.data

      })
    },

    getWeatherBySearch() {
      this.$axios(`${ this.apiUrl }?q=${this.search}&appid=${ this.apiKey }&units=metric`).then(response => {
        this.weatherData = response.data

      })
    },
  }
}
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to bottom, #136a8a, #267871)

  .degree
    top: -44px

  .skyline
    background: url("../assets/bg.png") center bottom
    flex: 0 0 100px
    background-size: contain

</style>
