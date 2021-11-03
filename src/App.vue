<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
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
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div  class="weather-wrap" v-else-if="typeof weather.main == 'undefined' && query"> 
        <div class="weather-box">
          <div class="msg">
            <h2 class="err"> Not Found </h2>
            <h6>can't find city : <span> {{ query }} </span></h6>
          </div>
        </div>
      </div>

      <div  class="weather-wrap" v-else> 
        <div class="weather-box">
          <div class="msg">
            <h2 class="welcom"> Welcom </h2>
            <h6>Enter Your City...</h6>
          </div>
            
        </div>
      </div>
    </main>
    <div class="footer">
       <dir>et-weather</dir> <div> <a href="http://sevenoon.pythonanywhere.com/about/" target="_blank"> sevenoon </a> </div> <div> AllRights Reserved 2021 </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '70a64d2311a897013f9adbe3abbde510',
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
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
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
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background: rgb(255, 145, 0);
}
main {
  min-height: 90vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(172, 160, 160, 0.25), rgba(0, 0, 0, .75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
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
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .msg{
  display: inline-block;
  align-self: center;
  justify-self: center;
  padding: 10px 25px;
  font-size: 6rem;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .msg h6{
  font-size: 23px;
  font-weight: 300;
  color:rgb(255, 81, 0) ;
  text-shadow: none;
}
.weather-box .msg .err{
    color: rgb(250, 44, 44);
}
.weather-box .msg .welcom{
  color: green;
}
.weather-box .msg span{
  color:teal;
}
.footer{
  height: 6rem;
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  align-items: center;
  font-family: 'Courier New', Courier, monospace;
}
@media only screen and (max-width: 600px) {
  .weather-box .msg{
    font-size: 2rem;
  font-weight: 900;
  }
}
</style>