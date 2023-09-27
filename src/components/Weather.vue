<template>
  <div class="container p-0">
    <div class="d-flex">
      <div class="card main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">Today</h2>
          <p class="text-light date mb-0">{{date}}</p>
          <small>{{time}}</small>
          <h2 class="place"><i class="fa fa-location">{{ name }} <small>{{country}}</small></i></h2>
          <div class="temp">
            <h1 class="weather-temp">{{temperature}}&deg;</h1>
            <h2 class="text-light">{{description}} <img :src="iconUrl"></h2>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>Feels Like</th>
              <td>{{ feels_like }}</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{ humidity }}</td>
            </tr>
            <tr>
              <th>Wind</th>
              <td>{{ wind }}</td>
            </tr>
          </tbody>
        </table>

        <days-weather :cityname="cityname"></days-weather>
        <div id="div_form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input type="button" value="Change Location" @click="changeLocation" class="btn change-btn btn-primary">
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import DaysWeather from './DaysWeather.vue';
export default {
  name: 'myWeather',
  components: {
    DaysWeather
  },
  props:{
    city:String,
  },
  data(){
    return{
      cityname:this.city,
      temperature: null,
      description:null,
      iconUrl: null,
      date: null,
      time: null,
      name: null,
      feels_like:null,
      wind:null,
      humidity:null,
      country:null,
      monthNames:["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November",  "December"],
    }
  },
  methods:{
    changeLocation(){
      window.location.reload();
    }
  },
  async created(){
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=d711a983698285e8eb88492a02c305ff`);
    const weatherData = response.data;
    this.temperature= Math.round(weatherData.main.temp);
    this.description=weatherData.weather[0].description;
    this.name = weatherData.name;
    this.wind = weatherData.wind.speed;
    this.feels_like = Math.round(weatherData.main.feels_like);
    this.country = weatherData.sys.country;
    this.humidity = weatherData.main.humidity;

    this.iconUrl=`https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
    const d = new Date();
    this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
    this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
    console.log(weatherData);
  }
}
</script>

<style scoped>

.weather-temp{
  margin: 0;
  font-weight: 700;
  font-size: 4em;
}

h2.mb-1.day{
  font-size: 3rem;
  font-weight: 400;
}

.main-div{
  border-radius:20px;
  color: #fff;
  background-image: url("https://images.unsplash.com/photo-1695327677553-aa69700a6fb7?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1471&q=80");
  background-size: cover;
  background-position: center;
  background-blend-mode: overlay;
  background-color: rgba(0, 0, 0, 0.5);
  background-repeat: no-repeat;
}


.temp{
  position: absolute;
  bottom: 0;
}

.main-div:hover{
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index:1;
}

.card-2{
  background-color: #212730;
  border-radius: 20px;
}

.card-details{
  margin-left: 19px;
}

.h1_Left{
  position:absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}

.h3_Left{
  position: absolute;
  left:16px;
  font-size: 2vw;
  line-height: 0.5;
}

.h3_Left small{
  font-size: 1rem;
}

table{
  position: relative;
  left: 15px;
  border-collapse: separate;
  border-spacing: 15px;
  width: 85%;
  text-align: left;
  max-width: 600px;
  margin: 0 auto;
}

th,
td{
  font-size: 18px;
  color: #fff;
}

td{
  text-align: right;
}

table,
tr:hover{
  color: red;
}


.change_btn{
  background-image: linear-gradient(to right, cyan, magenta);

}

.change_btn:hover{
  transform: scale(0.9);
  transition: transform 0.1s ease;
}
</style>
