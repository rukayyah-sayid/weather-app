<template>
  <div id="app" class="col-lg-12">
    <main>
      <header class="text-white text-center mt-3">
        <p class="font-46 font-weight-bold">Weather Forecast</p>
        <p style="margin-top: -15px">Type in the name of your city to check </p>
      </header>

      <div class="search-box mx-auto container-fluid mt-4">
        <input 
        type="text" 
        class="search search-bar col-lg-7 pt-2 pb-2" 
        placeholder="Search..." 
        v-model="query"
        @keypress= "fetchWeather"
        />
      </div>

      <div class="mt-3">
      <div class="weather-wrap col-lg-5" v-if="typeof weather.main != 'undefined'">
      <p class="location text-white font-32 text-center mt-3 pt-2"> {{ weather.name }}, {{ weather.sys.country }} </p>
      <p class="date font-weight-bold text-white pl-1 font-italic font-12 text-center m-0 p-0"> {{ dateBuild() }} </p>
      <div class="temperature mx-auto rounded">
        <p class="text-center mx-auto text-white font-46"> {{ Math.round(weather.main.temp)}}°C </p>
        <p class="text-white pl-1 font-italic font-12 text-center pb-2" style="margin-top: -20px;">{{ weather.weather[0].main }} </p>
      </div>
      </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',

  data() {
    return {
      api_url: "https://api.openweathermap.org/data/2.5/",
      api_key: 'd38d6362beb0b151935b03745081ca0e',
      query: '',
      weather: {}
    }
  },

  methods: {
    fetchWeather(e) {
      if( e.key == "Enter") {
        fetch(this.api_url + 'weather?q=' + this.query + '&units=metric&APPID=' + this.api_key).then(
          res => {
          return res.json()
        }).then(this.setResults)}
    },

    setResults (results){
      this.weather = results;
    },

    dateBuild(){
      let date = new Date()
      let months = ['January' , 'February' , 'March' , 'April' , 'May' , 'June' , 'July' , 'August' , 'September' , 'October' , 'Novemver' , 'December']
      let weeks = ['Monday' , ' Tuesday' , 'Wednesday' , 'Thursday' , 'Friday', 'Saturday' , 'Sunday']
      
      let current_date = date.getDate()
      let current_week = weeks[date.getDay()-1]
      let current_year = date.getFullYear()
      const current_month = months[date.getMonth()]

      return current_date + current_month + ',' + current_week + current_year 
    },

    timeBuild() {
      var today = new Date();
      var time = today.getHours() + ":" + today.getMinutes()

      return time
    }

  }
}
</script>

<style>

.font-12 {
  font-size: 12px;
}

.font-20 {
  font-size: 20px;
}

.font-32 {
  font-size: 32px;
}

.font-46 {
  font-size: 46px;
}

header {
  justify-content: center !important;
}

#app {
  background-size: cover;
  background-position: bottom;
   background-image: url('./assets/warm-weather.jpg');
  transition: 0.4s;
}

body {
  font-family: 'montserrat', sans-serif;
}

main {
  min-height: 100vh;
  padding: 25px;
}

.search-box {
  padding-left: 380px !important;
}

.search-bar {
  border-radius: 0px 12px 0px 12px;
  background: #c03d24;
  color: white;
  border: 2px solid white;
}

.search-bar::placeholder {
  font-size: 14px;
  color: white;
}

.search-bar::focus {
  border: 1px solid white !important;
  font-size: 14px;
  color: white;
}

.weather-wrap {
  background-size: cover;
  background-position: bottom;
  background: #c03d24;
  border-radius: 12px;
  border: 2px solid white;
  height: 240px;
  margin: 0px auto !important;
}

.date {
  text-shadow: 1px 2px #97301b;
}

.location {
  text-shadow: 1px 2px #97301b;
}

.temperature {
  font-weight:700;
  text-shadow: 1px 2px #97301b;
  width: 140px;
}

</style>
