<template>

  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm': ''">

    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search.." v-model="query" @keypress="fetchWeather" />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            <h1> {{ weather.name }}, {{ weather.sys.country }}</h1>
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp)}}</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>

  </div>

</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        api_key: '98eda5f9e05dd5ccbc47b3a38ae5994f',
        base_url: 'https://api.openweathermap.org/data/2.5/',
        query: '',
        weather: {}
      }
    },
    methods: {
      fetchWeather(e) {
        if (e.key == "Enter") {
          let fullkey = `${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`;
          fetch(fullkey)
            .then(res => {
               console.log(fullkey);
              return res.json();
            }).then(this.setResults);
        }
      },
      setResults(results) {
        this.weather = results;
        console.log(results);
      },
      dateBuilder(){
       let d = new Date();
       let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
       let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

       let day = days[d.getDay()];
       let date = d.getDate();
       let month = months[d.getMonth()];
       let year = d.getFullYear();
       
       return `${day} ${date} ${month} ${year}`;
      }
    }
  }
</script>

<style>
#app.warm{
  background-color: orange;
}
</style>