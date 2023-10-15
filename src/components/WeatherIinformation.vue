<template>
    <div class="container">
        <div class="innercontainer">
            <h2>{{ temperature }}<span>&#176;</span></h2>
            <p>{{ weatherDescription }}</p>

            <div v-if="showCloudy" class="weather-condition">
                <img src="../assets/cloudy.png" alt="">
            </div>
            <div v-if="showRainy" class="weather-condition">
                <img src="../assets/rain.png" alt="">
            </div>
            <div v-if="showStormy" class="weather-condition">
                <img src="../assets/storm.png" alt="">
            </div>
            <div v-if="showClear" class="weather-condition">
                <img src="../assets/clear.png" alt="">
            </div>
            <div v-if="showDrizzle" class="weather-condition">
                <img src="../assets/drizzle.png" alt="">
            </div>
            <div v-if="showSnow" class="weather-condition">
                <img src="../assets/snow.png" alt="">
            </div>
            <div v-if="showAtmosphere" class="weather-condition">
                <img src="../assets/atmosphere.png" alt="">
            </div>

            <div class="input-container">
                <label for="latitude">Latitude</label>
                <input type="text" name="latitude" v-model="latitude">
            </div>
            <div class="input-container">
                <label for="longitude">Longitude</label>
                <input type="text" name="longitude" v-model="longitude">
            </div>
            <button @click="GetWeatherData()">Get Data</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
    export default {
        data(){
            return{
                weather:{},
                weatherDescription: '',
                showCloudy: false,
                showRainy: false,
                showStormy: false,
                showClear: false,
                showDrizzle: false,
                showSnow: false,
                showAtmosphere: false,
                temperature: '',
                latitude: '18.7965',
                longitude: '98.6601'
            }   

        },
    methods:{
        GetWeatherData(){
            axios.get('https://api.openweathermap.org/data/2.5/weather?lat='+ this.latitude +'&lon='+ this.longitude +'&appid=c2f978dadbf21301d2cede6505dfa1ad').then(
                response => {
                    console.log(response.data.weather)
                    let mainDescription = response.data.weather[0].main;
                    let descriptionString = response.data.weather[0].description;
                    this.weatherDescription = descriptionString.charAt(0).toUpperCase() + descriptionString.slice(1);
                    
                    this.temperature = response.data.main.temp;

                    switch(true){
                        case mainDescription == 'Clouds':
                            this.showCloudy = true;
                            this.showStormy = false;
                            this.showRainy = false;
                            this.showClear = false;
                            this.showDrizzle = false;
                            this.showSnow = false;
                            this.showAtmosphere = false;
                            break;
                        case mainDescription == 'Thunderstorm':
                            this.showCloudy = false;
                            this.showStormy = true;
                            this.showRainy = false;
                            this.showClear = false;
                            this.showDrizzle = false;
                            this.showSnow = false;
                            this.showAtmosphere = false;
                            break;
                        case mainDescription == 'Rain':
                            this.showCloudy = false;
                            this.showStormy = false;
                            this.showRainy = true;
                            this.showClear = false;
                            this.showDrizzle = false;
                            this.showSnow = false;
                            this.showAtmosphere = false;
                            break;
                        case mainDescription == 'Clear':
                            this.showCloudy = false;
                            this.showStormy = false;
                            this.showRainy = false;
                            this.showClear = true;
                            this.showDrizzle = false;
                            this.showSnow = false;
                            this.showAtmosphere = false;
                            break;
                        case mainDescription == 'Drizzle':
                            this.showCloudy = false;
                            this.showStormy = false;
                            this.showRainy = false;
                            this.showClear = false;
                            this.showDrizzle = true;
                            this.showSnow = false;
                            this.showAtmosphere = false;
                            break;
                        case mainDescription == 'Snow':
                            this.showCloudy = false;
                            this.showStormy = false;
                            this.showRainy = false;
                            this.showClear = false;
                            this.showDrizzle = false;
                            this.showSnow = true;
                            this.showAtmosphere = false;
                            break;
                        case mainDescription == 'Atmosphere':
                            this.showCloudy = false;
                            this.showStormy = false;
                            this.showRainy = false;
                            this.showClear = false;
                            this.showDrizzle = false;
                            this.showSnow = false;
                            this.showAtmosphere = true;
                            break;
                    }
                }
            ).catch(error => {console.log(error)})
        }
    },
    mounted(){
        this.GetWeatherData();
    }
}
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f2f2f2;
}

.inner-container {
  text-align: center;
  background-color: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

h2 {
  font-size: 3rem;
  margin: 0;
}

span {
  font-size: 1.5rem;
}

p.description {
  font-size: 1.2rem;
  color: #555;
  margin-bottom: 20px;
}

.weather-condition img {
  max-width: 100px;
  margin: 10px;
}

.input-container {
  margin-top: 20px;
}

input {
  width: 100%;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0056b3;
}
</style>