<template>
  <div id="app" :class="temp">
    <div class="search">
      <input type="text" id="search-bar" 
       placeholder="Search: ex. City, (abbreviated Country ie. US--optional)"
       v-model="apiQuery"
       @keypress="getWeather">
    </div>

    <div class="weather-container" v-if="typeof currentWeather.main !== 'undefined'">
      <div class="place-text">{{ currentWeather.name }}, {{ currentWeather.sys.country }}</div>
      <div class="time-text">{{ getDate() }}</div>
      <div class="temp">{{ Math.round(currentWeather.main.temp )}}°F</div>
      <div class="weather">{{ currentWeather.weather[0].main }}</div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api: 'e6620fd9081a23cc5aed1ebaf174c2e2',
      apiURL: 'http://api.openweathermap.org/data/2.5/',
      apiQuery: '',
      currentWeather: {},
      temp: '',
    }
  },
  methods: {

    getWeather(event) {
      if (event.key == "Enter") {
        fetch(`${this.apiURL}weather?q=${this.apiQuery}&units=imperial&APPID=${this.api}`)
          .then(response => {
            return response.json();
          }).then(this.setWeather);
      }
    },

    setWeather(weather) {
      this.currentWeather = weather;

      if (this.currentWeather.main.temp > 80) {
        this.temp = 'hot';
      } else if (this.currentWeather.main.temp < 50) {
        this.temp = 'cold';
      } else {
        this.temp = 'norm';
      }
    },

    getDate () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day}, ${month} ${date} ${year}`;
    },
  }
}
</script>

<style>

* {
  margin: 0;
  padding: 0;
}

#app {
  background-image: url("./assets/weather-norm.jpg");
  min-height: 100vh;
  background-size: cover;
  background-position: center;
  font-family: sans-serif;
}

#app.hot {
  background-image: url("./assets/weather-hot.jpg");
}

#app.cold {
  background-image: url("./assets/weather-cold.jpg");
}

#app.norm {
  background-image: url("./assets/weather-norm.jpg");
}

.search, .place-text, .time-text, .temp, .weather {
  display: flex;
  justify-content: center;
}

.search {
  padding: 50px;
}

#search-bar {
  width: 80vw;
  height: 6.5vh;
  border: none;
  border-radius: 10px;
  padding-left: 10px;
  position: absolute;
  background-color: rgba(255, 255, 255, .75);
  box-shadow: 0px 6px 5px rgba(23, 0, 24, 0.6);
  transition: 200ms;
  font-size: medium;
}

#search-bar:hover {
  transform: scale(1.03);
  background-color: white;
  cursor: pointer;
}

#search-bar:focus {
  outline: none;
  background-color: white;
}

.place-text, .time-text, .temp, .weather {
  color: white;
  text-shadow: 4px 4px 6px rgba(25, 1, 27, 0.7);
}

.place-text, .weather {
  font-size: 6vh;
}

.place-text {
  margin-top: 4vh;
}

.time-text {
  opacity: .7;
  font-size: large;
  font-style: italic;
}

.temp {
  margin: 4vh 30vw 3.5vh 30vw;
  height: 20vh;
  background-color:rgba(255, 255, 255, .7);
  border-radius: 30px;
  align-items: center;
  font-size: 12vh;
  box-shadow: 3px 6px rgba(23, 0, 24, .75);
  font-weight: 600;
}

.weather {
  font-style: italic;
  font-weight: 600;
}

</style>
