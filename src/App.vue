<script setup>
import { ref, onMounted } from 'vue'
import axios from "axios";

let lat = ref('');
let lon = ref('');
let city = ref('');
let country = ref('');
let temperature = ref('');
let humidity = ref('');
let description = ref('');
let wind_speed = ref('');
let icon = ref('');
let ipinfoApiKey = import.meta.env.VITE_IPINFO_API_KEY
let openWeatherMapApikey = import.meta.env.VITE_OPENWEATHERMAP_API_KEY

onMounted(async () => {
  await axios.get(`https://ipinfo.io/json?token=${ipinfoApiKey}`).then((response =>{
    lat.value = response.data.loc.split(',')[0];
    lon.value = response.data.loc.split(',')[1];
    city.value = response.data.city;
    country.value = response.data.country;
  })).then(async () =>{
    await axios.get(`https://api.openweathermap.org/data/2.5/onecall?lat=${lat.value}&lon=${lon.value}&units=metric&appid=${openWeatherMapApikey}`)
        .then(response =>{
          temperature.value = response.data.current.temp;
          humidity.value = response.data.current.humidity;
          description.value = response.data.current.weather[0].description;
          wind_speed.value = response.data.current.wind_speed;
          icon.value = `http://openweathermap.org/img/wn/${response.data.current.weather[0].icon}@2x.png`;
        })
  })
});

</script>

<template>


  <section class="content">
    <p>{{ city }} / {{ country }}</p>
    <p>Temperature: {{ temperature }}°C</p>
    <p>Humidity: {{ humidity }}%</p>
    <p>Description: {{ description }}</p>
    <p>Wind speed: {{ wind_speed }} km/h</p>
    <p><img :src="icon" alt="weather icon" /></p>
  </section>

</template>

<style scoped>
</style>
