<template>
  <div>
    <NuxtRouteAnnouncer />
    <div class="h-[80px] bg-[#4254E0] w-full flex justify-center items-center space-x-[100px] p-2">
      <div class="flex justify-center items-center ">
        <h2 class="text-[#FFFB00] text-[30px]">Meteo</h2>
        <h2 class="text-[30px] text-[#FFFFFF]">Predict</h2>
      </div>
      <div class="hidden md:flex justify-center space-x-[40%] items-center">
        <h1 class="animate-pulse">To see the result please write the real cities in searcher</h1>
      </div>
    </div>
    <div class="flex justify-center mt-[5%]">
      <div class="w-[90%] md:w-[600px] h-[700px] drop-shadow-lg bg-gradient-to-r from-green-400 to-blue-500 rounded-md" id="card">
        <div class="flex justify-center mt-[10%]">
          <input v-model="city" type="text" class="w-[70%] h-[50px] border-[1px] border-[#000000] rounded-[50px] pl-[3%]" placeholder="please enter city..">
          <div class="w-[50px] h-[50px] rounded-[100px] bg-[#FFFFFF] border-[1px] border-[#000000] flex justify-center items-center ml-[2%]">
            <img src="/search.webp" alt="" @click="checkWeather" class="w-[30px] h-[30px]">
          </div>
        </div>
        <div class="flex justify-center mt-[20px] items-center space-y-[1%] flex-col">
          <div id="city" class="w-full h-[100px] flex items-center justify-center ">
            <h1 class="text-[40px]">{{ weather.name }}</h1>
          </div>
          <div id="description" class="">
            <img :src="weatherImage" alt="Weather Image" class="w-[200px] h-[200px] object-cover rounded-md animate-box">
            <h1 class="flex justify-center text-[30px]">{{ weather.weather[0].description }}</h1>
          </div>
          <div id="temp" class="flex justify-center w-[200px] h-[30px]">
            <h1>Temperature: {{ floor }}°C</h1>
          </div>
          <div id="humidity">
            <h1>Humidity: {{ weather.main.humidity }}%</h1>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      weather: {
        name: 'City',
        main: {
          temp: '',
          humidity: ' '
        },
        weather: [
          {
            description: '' // This will be updated with actual data
          }
        ]
      }
    };
  },
  methods: {
    async checkWeather() {
       const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${useRuntimeConfig().public.apiKey}`;
      try {
        const response = await fetch(apiUrl);
        if (!response.ok) {
          throw new Error('City not found');
        }
        const data = await response.json();
        this.weather = data;
      } catch (error) {
        alert(error.message);
      }
    },
    getWeatherImage(description) {
      if (description.includes('clear')) {
        return '/sun.webp';  // Corrected path
      } else if (description.includes('clouds')) {
        return '/cloudy.webp';  // Corrected path
      } else if (description.includes('rain')) {
        return '/rain.webp';  // Corrected path
      } else if (description.includes('storm')) {
        return '/storm.webp';  // Corrected path
      } else if (description.includes('snow')) {
        return '/snow.webp';  // Corrected path
      } else if (description.includes('fog')) {
        return '/fog.webp';  // Corrected path
      } else if (description.includes('mist')) {
        return '/fog.webp';  // Corrected path
      } else if (description.includes('haze')) {
        return '/fog.webp';  // Corrected path
      } else if (description.includes('drizzle')) {
        return '/cloudy.webp';  // Corrected path
      }
      else {
        return '/question.webp';  // Corrected path
      }
    }
  },
  computed: {
    weatherImage() {
      const description = this.weather.weather.length > 0 ? this.weather.weather[0].description : '';
      return this.getWeatherImage(description);
    },
    floor() {
      return Math.floor(this.weather.main.temp - 273);
    }
  }
};
</script>

<style scoped>
h1 {
  color: white;
}
.animate-box {
    
    animation: moveHorizontally 5s ease-in-out ;
}

@keyframes moveHorizontally {
    0% {
        transform: translateX(0);
    }
    50% {
        transform: translateX(30px); /* Move 200px to the right */
    }
    100% {
        transform: translateX(0);
    }
}
</style>
