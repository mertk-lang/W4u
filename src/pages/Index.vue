<template>
  <q-page class="flex column" :class="bgClass">
     <div class="col q-pa-lg">
        <q-input @click="nullifyError" v-model="search" @keyup.enter="getWeatherBySearch" dark borderless placeholder="Label">
          <template v-slot:before>
            <q-icon name="my_location" @click="getWeatherByLocation" />
          </template>

          <template v-slot:append>
            <q-icon @click="getWeatherBySearch" name="search" />
          </template>
          </q-input>
          <q-banner v-if="this.errorMessage" inline-actions class="q-mt-md text-white bg-red">
            {{errorMessage}}
          </q-banner>
     </div>

     <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          {{weatherData.name}}
        </div>
        <div class="text-h6 text-weight-light q-my-sm">
          {{weatherData.weather[0].description}}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          {{Math.round(weatherData.main.temp)}}<span class="text-h3 degree relative-position ">&deg;</span>
        </div>
      </div>

      <div class="col text-center">
        <img :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`">
      </div>
     </template>

     <template v-else>
       <div class="col column text-center text-white position-relative">
         <div class="col text-h2 text-weight-thin">
           What a lovely night
         </div>
          <q-btn @click="getWeatherByLocation" class="col location position-relative" size="1em" flat>
            <q-icon left size="2em" name="my_location" />
            <div>Find my location</div>
          </q-btn>
       </div>
     </template>

  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: '',
      errorMessage: '',
      lat: '',
      lon: '',
      apiKey: 'db065681ac6cf240c4442a7ce130ce20'
    }
  },
  computed: {
    bgClass() {
      if(this.weatherData){
        if(this.weatherData.weather[0].icon.endsWith('n')) {
          return 'bg-night';
        }
        else {
          return 'bg-day';
        }
      }
    }
  },
  methods: {
    getWeatherByLocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeather()
        this.errorMessage = null;
      }, (error) => {
        this.errorMessage = error.message;
      }, { timeout: 8000})
    },
    getWeather() {
      this.$q.loading.show();
      let API = `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`;
      this.$axios.get(API)
      .then((res) => {
        this.weatherData = res.data;
        this.errorMessage = null;
        this.$q.loading.hide();
      })
      .catch((err) => {
        this.errorMessage = err.message;
        this.$q.loading.hide();
      })
    },
    getWeatherBySearch() {
      this.$q.loading.show();
       let API = `https://api.openweathermap.org/data/2.5/weather?q=${this.search}&appid=${this.apiKey}&units=metric`;
      this.$axios.get(API)
      .then((res) => {
        this.weatherData = res.data;
        this.errorMessage = null;
        this.$q.loading.hide();
      })
      .catch((err) => {
        this.errorMessage = `Couldn't find there, sorry`
        this.$q.loading.hide();
      })
    },
    nullifyError() {
      this.errorMessage = '';
    }
  }
}
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to bottom, #43cea2, #185a9d)
    &.bg-day
      background: linear-gradient(to top, #4e54c8, #8f94fb)
    &.bg-night
      background: linear-gradient(to top, #0f2027, #203a43, #2c5364)

  .degree
    top: -38px

  .location
    top: -70px

</style>
