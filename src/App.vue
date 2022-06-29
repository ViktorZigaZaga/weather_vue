<template>
  <div id="app" :class="typeof weather.name != 'undefined'">

    <main>
      <h1>Узнай погоду в своем городе</h1>
      <div class="search-box">
        <form @submit.prevent>
          <div v-if ="!isWeatherLoading" >
            <input
              class="search-bar"
              type="text"
              aria-label="search"
              placeholder="Введите название города"
              v-bind:value="queryCity"
              @input="queryCity = $event.target.value"
            >
            <button class="btn" @click="createWeather">Поиск</button>
          </div>
          <div class="spinner" v-else></div>
        </form>
      </div>

      <div class="weather-wrap" v-if ="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location"> {{ weather.name }} , {{ weather.sys.country }}</div>
          <div class="date"> {{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp"> {{ Math.round(weather.main.temp)}} °C</div>
          <div id="weather" :class="weather.weather[0].main == 'Clear' ? 'clear'
                                  : weather.weather[0].main == 'Error' ? 'error'
                                  : weather.weather[0].main == 'Clouds' ? 'clouds'
                                  : weather.weather[0].main == 'Fog' ? 'fog'
                                  : weather.weather[0].main == 'Storm' ? 'storm'
                                  : weather.weather[0].main == 'Snow' ? 'snow'
                                  : weather.weather[0].main == 'Rain' ? 'rain' : 'weather'">
            {{weather.weather[0].main}}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'app',
  data() {
    return {
      apiKey: '01324404c6b4ed6bcbfeaccda98b7710',
      urlOWN: 'https://api.openweathermap.org/data/2.5/',
      queryCity: '',
      errored: false,
      weather: {},
      isWeatherLoading: false,
    };
  },

  methods: {
    async createWeather() {
      try {
        this.isWeatherLoading = true;
        setTimeout(async () => {
          const response = await axios.get(`${this.urlOWN}weather?q=${this.queryCity}&units=metric&APPID=${this.apiKey}`);
          this.setResults(response.data);
          this.isWeatherLoading = false;
        }, 1000);
      } catch (error) {
        console.log(error);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      const d = new Date();
      const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      const day = days[d.getDay()];
      const date = d.getDate();
      const month = months[d.getMonth()];
      const year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
h1{
  padding: 20px;
  font-weight: 500;
  color: #313131;
  text-align: center;
}
body{
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
#app{
  background-image: url('./images/startfon.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.error {
  background-image: url('./images/error.jpg');
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
  margin-right: 0;

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
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.btn {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  margin-right: 0;
  right: 30px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(238,250,105,0.5);
  border-radius: 0 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 0px 16px 0px 16px;
}
.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 400;
  font-style: italic;
  text-align: center;
}
.weather-box{
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 800;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
#weather{
  display: flex;
  position: absolute;
  left: 35%;
  top: 65%;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 800;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
#weather.clear {
  background-image: url('./images/warm.jpg');
}
#weather.clouds {
  background-image: url('./images/clouds.jpg');
}
#weather.rain {
  background-image: url('./images/rain.jpg');
}
#weather.storm {
  background-image: url('./images/storm.jpg');
}
#weather.snow {
  background-image: url('./images/snow.jpg');
}
div{ margin: 10px;  }
.spinner{
  width: 100px;
  height: 100px;
  display: flex;
  flex-direction: column;
  align-self: center;
  border: 5px solid transparent;
  border-left: 3px solid #05cbf7;
  border-right: 3px solid #05cbf7;
  position: fixed;
  border-radius: 50px;
  left: 0;
  right: 0;
  margin: 0 auto;
  animation: spin 1s linear infinite
}
@keyframes spin{
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

</style>
