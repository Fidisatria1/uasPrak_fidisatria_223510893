<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="bg-gradient">
      <q-toolbar>
        <q-toolbar-title class="text-white">Informasi Cuaca</q-toolbar-title>
        <div class="current-time text-white">{{ currentTime }}</div>

        <q-btn-group>
          <q-btn label="Daftar Tugas" color="grey" class="bg-grey">
            <q-menu auto-close>
              <q-list style="min-width: 200px">
                <q-item
                  to="/tugas/1"
                  href="https://vercel.com/fidisatria-s-projects/pertemuan1-fidi-satria-223510893/4fRE34WDZb9sXWCm8LrV9NZQHdz3"
                  >Tugas Pertemuan 1</q-item
                >
                <q-item to="/tugas/2" href="https://github.com/Fidisatria1/223510893-Fidi"
                  >Tugas Pertemuan 2</q-item
                >
                <q-item to="/tugas/3" href="https://vercel.com/fidisatria-s-projects/fidisatria-223510893"
                  >Tugas Pertemuan 3</q-item
                >
                <q-item to="/tugas/4" href="https://vercel.com/fidisatria-s-projects/223510893-fidisatria-tugas4"
                  >Tugas Pertemuan 4</q-item
                >
                <q-item
                  to="/tugas/5"
                  href="https://vercel.com/fidisatria-s-projects/fidisatria-223510893-5"
                  >Tugas Pertemuan 5</q-item
                >
                <q-item
                  to="/tugas/6"
                  href="https://vercel.com/fidisatria-s-projects/fidisatria-223510893-tugas6"
                  >Tugas Pertemuan 6</q-item
                >
    
                >
              </q-list>
            </q-menu>
          </q-btn>
        </q-btn-group>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page class="flex flex-center">
        <q-card class="q-pa-md glass-card">
          <q-card-section>
            <div class="text-h6 text-center text-primary">
              <strong>CUACA HARI INI DI SELURUH NEGARA</strong>
            </div>
          </q-card-section>

          <q-card-section>
            <q-input
              v-model="location"
              outlined
              placeholder="Masukkan lokasi"
              class="input-field"
              @keyup.enter="fetchWeather"
            >
              <template v-slot:append>
                <q-btn label="Cari" color="primary" @click="fetchWeather" />
              </template>
            </q-input>
          </q-card-section>

          <q-card-section v-if="weather" class="weather-info">
            <div class="text-center text-black">
              <strong>Lokasi:</strong> {{ weather.name }}
            </div>
            <div :class="temperatureClass" class="text-center">
              <strong>Temperatur:</strong> {{ weather.main.temp }}°C
            </div>
            <div class="text-center text-black">
              <strong>Deskripsi:</strong> {{ weather.weather[0].description }}
            </div>
          </q-card-section>
        </q-card>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { ref, watch, onMounted } from "vue";
import axios from "axios";

export default {
  name: "App",
  setup() {
    const location = ref("");
    const weather = ref(null);
    const rightDrawerOpen = ref(false);
    const apiKey = "8a7436bc7a340da337b17310ea4fd29a";
    const currentTime = ref(new Date().toLocaleTimeString());

    const fetchWeather = async () => {
      if (location.value) {
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${location.value}&appid=${apiKey}&units=metric`;
        try {
          const response = await axios.get(apiUrl);
          weather.value = response.data;
        } catch (error) {
          console.error(error);
        }
      }
    };

    const temperatureClass = ref("");

    // Watch weather changes to update temperatureClass
    watch(weather, (newVal) => {
      if (newVal && newVal.main.temp) {
        const temp = newVal.main.temp;
        if (temp > 30) {
          temperatureClass.value = "text-red";
        } else if (temp >= 25 && temp <= 30) {
          temperatureClass.value = "text-yellow";
        } else {
          temperatureClass.value = "text-green";
        }
      }
    });

    // Update current time every second
    const updateTime = () => {
      currentTime.value = new Date().toLocaleTimeString();
    };
    setInterval(updateTime, 1000);

    const toggleRightDrawer = () => {
      rightDrawerOpen.value = !rightDrawerOpen.value;
    };

    // Fetch weather data on component mount (optional)
    onMounted(() => {
      fetchWeather();
    });

    return {
      location,
      weather,
      rightDrawerOpen,
      fetchWeather,
      toggleRightDrawer,
      temperatureClass,
      currentTime,
    };
  },
};
</script>

<style>
.text-red {
  color: red;
}

.text-yellow {
  color: yellow;
}

.text-green {
  color: green;
}

body {
  font-family: "Roboto", sans-serif;
  background: linear-gradient(175deg, #5a87a1, #76b2ce);
  color: #141212;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.bg-gradient {
  background: linear-gradient(90deg, #3e6974, #4c6680);
}

.glass-card {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  border: 1px solid rgba(255, 255, 255, 0.18);
  transition: transform 0.2s;
}

.glass-card:hover {
  transform: scale(1.05);
}

.glass-drawer {
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  border: 1px solid rgba(255, 255, 255, 0.18);
}

.text-white {
  color: rgb(255, 255, 255);
}

.text-primary {
  color: #00bcd4;
}

.input-field .q-field__control {
  background: rgba(218, 218, 218, 0.2);
  border-radius: 5px;
  color: white;
}

.input-field .q-field__control:before,
.input-field .q-field__control:after {
  border-color: rgba(255, 255, 255, 0.3);
}

.input-field .q-input__inner {
  color: white;
}

.weather-info {
  margin-top: 20px;
  animation: fadeIn 1s ease-in-out;
}

.current-time {
  margin-left: auto;
  padding-right: 20px;
}

.bg-black .q-menu {
  background-color: black;
}
</style>
