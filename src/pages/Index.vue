<template>
  <q-page class="flex column">
     <div class="col q-pa-lg">
        <q-input v-model="search" dark borderless placeholder="Label">
          <template v-slot:before>
            <q-icon name="my_location" />
          </template>

          <template v-slot:append>
            <q-icon name="search" />
          </template>
          </q-input>
     </div>

     <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          Tokyo
        </div>
        <div class="text-h6 text-weight-light q-my-sm">
          Rainy
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          4<span class="text-h3 degree relative-position ">&deg;</span>
        </div>
      </div>

      <div class="col text-center">
        <img src="https://www.fillmurray.com/100/100" alt="">
      </div>
     </template>

     <template v-else>
       <div class="col column text-center text-white position-relative">
         <div class="col text-h2 text-weight-thin">
            What a lovely night
         </div>
          <q-btn class="col location position-relative" size="1em" flat>
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
      weatherData: ''
    }
  }
}
</script>

<style lang="sass">
  .q-page
    background: linear-gradient(to top, #4e54c8, #8f94fb)

  .degree
    top: -38px

  .location
    top: -70px

</style>
