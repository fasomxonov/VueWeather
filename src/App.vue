<template>
  <div id="app" :class="typeof weather.main !='undefined' && weather.main.temp > 16 ? 'warm' : '' ">
    <main>
      <div class="search_box">
        <input
        type="text"
        class="search_bar"
        placeholder="Search..."
        v-model="query"
        @keypress="showWeather"
        >
      </div>

      <div class="weather_wrap" v-if="typeof weather.main !='undefined'">
        <div class="location_box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather_box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
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
      api_key: '985212bece0ceaa1c3361138149146e6',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    showWeather(e) {
      if(e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults)
      }
    },
    setResults(results) {
      this.weather = results
    },
    dateBuilder() {
      let d = new Date()
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "December"]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-image: url('@/assets/cold.jpg');
  background-size: cover;
  background-position: center;
  transition: .4s all;
}
#app.warm {
  background-image: url('@/assets/warm.jpg');
}
main {
  display: grid;
  grid-template-columns: 50% 50%;
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,.25), rgba(0,0,0,6));
}
.search_box {
  width: 90%;
}
.search_bar {
  margin-top: 30vh;
  display: block;
  width: 100%;
  padding: 15px;
  color: #ebebeb;
  font-size: 20px;
  background-color: rgba(204, 204, 204, 0.356);
  outline: none;
  appearance: none;
  border-radius: 30px 0px 30px 0px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.226);
  transition: .4s all;
  border-top-color: #8d8c8c9d;
  border-left-color: #8d8c8c9d;
}
.search_bar::placeholder {
  color: rgba(197, 209, 209, 0.603);
}
.search_bar:active, .search_bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.473);
  background-color: rgba(204, 204, 204, 0.456);
}
.location {
  margin-top: 10vh;
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.226);
}
.date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather_box {
  text-align: center;
}
.temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.226);
  background-color: rgba(197, 209, 209, 0.603);
  border-radius: 16px;
  margin: 30px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.226);
}
.weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.226);
}
</style>