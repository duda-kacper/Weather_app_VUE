<template>
<div :key="weather.dt" id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20  ?
'warm' : ''">
  <main>
    <div class="search-box">
      <input 
      type="text" 
      class="search-bar" 
      placeholder="Search..."
      v-model="query"
      @keypress="fetchWeather"
      />
    </div>

    <div v-if="error" class="error-message">{{ error }}</div>

    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date">{{ dateBuilder () }}</div>
      </div>

      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather">{{ weather.weather [0]. main}}</div>
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
    api_key: 'b7c92c912b14fa6b69febab959549870',
    url_base: 'https://api.openweathermap.org/data/2.5/',
    query: '',
    weather: {}
  }
},
methods: {
  fetchWeather (e) {
    if (e.key == "Enter") {
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
      .then(res => {
        if (!res.ok) {
              throw new Error('CITY NOT FOUND');
            }
            return res.json();
          })
          .then(this.setResults)
          .catch(err => {
            this.weather = {}
            this.error = err.message;
          });
      }
    },
    setResults(results) {
      this.weather = results;
      this.error = null;
  },
  dateBuilder () {
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
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body{
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-image: url('./assets/cold-bg.png');
    background-size: cover;
    background-position: center;
    transition: background-image 0.4s ease-in-out;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.png');
    transition: background-image 0.4s ease-in-out;
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  .search-box {
    width:100%;
    margin-bottom: 30px;
  }
  .search-box .search-bar{
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }
  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
  }

  .error-message {
    width: 100%;
    height: 50px;
    padding: 10px;
    border: 5px solid rgba(249, 24, 24, 0.668);
    display: flex;
    align-items: center;
    justify-content: center;
    color: rgba(249, 24, 24, 0.668);
    text-align: center;
    margin: 20px 0;
    font-size: 24px;
    border-radius: 0px 16px 0px 16px;
  }

.location-box .location {
  color: #FFFFFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25)
}

.location-box .date {
  color: #FFFFFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #FFFFFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25)
}

.weather-box .weather {
  color: #ffffff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
