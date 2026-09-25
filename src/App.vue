<template>
  <div class="app">
    <h1 class="title">🌤️ Lania Weather App</h1>

    <!-- SearchBar avec v-model + props/events -->
    <SearchBar
      :modelValue="city"
      @update:modelValue="city = $event"
      @search="searchWeather"
      :loading="loading"
    />

    <!-- Loading state -->
    <LoadingMessage v-if="loading" message="Fetching weather data..." />

    <!-- Error state -->
    <div v-else-if="error" class="error-box">
      <p>❌ {{ error }}</p>
    </div>

    <!-- Success state -->
    <WeatherCard v-else-if="weather" :weather="weather" :cityName="cityName" />

    <!-- Welcome state -->
    <div v-else class="welcome">
      <p>🔍 Search for a city to see its weather</p>
      <p class="examples">Try: Bujumbura, Tokyo, Nairobi, Shanghai, Paris</p>
    </div>

    <!-- v-show exemple : badge de statut -->
    <p v-show="weather && !loading" class="status-badge">
      ✅ Last search: {{ cityName }}
    </p>
  </div>
</template>

<script>
import SearchBar from './components/SearchBar.vue'
import WeatherCard from './components/WeatherCard.vue'
import LoadingMessage from './components/LoadingMessage.vue'

export default {
  name: 'App',
  components: { SearchBar, WeatherCard, LoadingMessage },

  data() {
    return {
      city: '',
      cityName: '',
      weather: null,
      loading: false,
      error: ''
    }
  },

  methods: {
    async searchWeather() {
      const query = this.city.trim()
      if (!query) {
        this.error = 'Please enter a city name.'
        return
      }

      this.loading = true
      this.error = ''
      this.weather = null

      try {
        // 1. Geocoding - obtenir les coordonnées
        const geoRes = await fetch(
          `https://geocoding-api.open-meteo.com/v1/search?name=${encodeURIComponent(query)}&count=1&language=en&format=json`
        )
        const geoData = await geoRes.json()

        if (!geoData.results || geoData.results.length === 0) {
          throw new Error('CITY_NOT_FOUND')
        }

        const { latitude, longitude, name, country } = geoData.results[0]
        this.cityName = `${name}, ${country}`

        // 2. Weather API - obtenir la météo
        const weatherRes = await fetch(
          `https://api.open-meteo.com/v1/forecast?latitude=${latitude}&longitude=${longitude}&current=temperature_2m,relative_humidity_2m,wind_speed_10m,weather_code&timezone=auto`
        )

        if (!weatherRes.ok) throw new Error('API_ERROR')

        const weatherData = await weatherRes.json()
        const current = weatherData.current

        this.weather = {
          temperature: current.temperature_2m,
          humidity: current.relative_humidity_2m,
          windSpeed: current.wind_speed_10m,
          weatherCode: current.weather_code,
          icon: this.getWeatherIcon(current.weather_code),
          condition: this.getWeatherCondition(current.weather_code)
        }
      } catch (err) {
        if (err.message === 'CITY_NOT_FOUND') {
          this.error = `City "${query}" not found. Please try another city.`
        } else if (err.message === 'API_ERROR') {
          this.error = 'Weather service is unavailable. Please try again later.'
        } else {
          this.error = 'Network error. Please check your connection.'
        }
      } finally {
        this.loading = false
      }
    },

    getWeatherIcon(code) {
      const icons = {
        0: '☀️', 1: '🌤️', 2: '⛅', 3: '☁️',
        45: '🌫️', 48: '🌫️',
        51: '🌦️', 53: '🌦️', 55: '🌧️',
        61: '🌧️', 63: '🌧️', 65: '🌧️',
        71: '❄️', 73: '❄️', 75: '❄️',
        80: '🌦️', 81: '🌧️', 82: '⛈️',
        95: '⛈️', 96: '⛈️', 99: '⛈️'
      }
      return icons[code] || '🌡️'
    },

    getWeatherCondition(code) {
      const conditions = {
        0: 'Clear sky', 1: 'Mainly clear', 2: 'Partly cloudy', 3: 'Overcast',
        45: 'Foggy', 48: 'Depositing rime fog',
        51: 'Light drizzle', 53: 'Moderate drizzle', 55: 'Dense drizzle',
        61: 'Slight rain', 63: 'Moderate rain', 65: 'Heavy rain',
        71: 'Slight snow', 73: 'Moderate snow', 75: 'Heavy snow',
        80: 'Rain showers', 81: 'Moderate showers', 82: 'Violent showers',
        95: 'Thunderstorm', 96: 'Thunderstorm with hail', 99: 'Severe thunderstorm'
      }
      return conditions[code] || 'Unknown'
    }
  }
}
</script>

<style scoped>
.app {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(15px);
  border-radius: 20px;
  padding: 35px 30px;
  width: 100%;
  max-width: 500px;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.25);
  text-align: center;
  color: white;
}

.title {
  font-size: 1.8rem;
  margin-bottom: 25px;
  text-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}

.welcome {
  padding: 30px 10px;
  font-size: 1.05rem;
}

.examples {
  margin-top: 12px;
  font-size: 0.9rem;
  opacity: 0.85;
}

.error-box {
  background: rgba(255, 80, 80, 0.25);
  border: 1px solid rgba(255, 80, 80, 0.6);
  padding: 18px;
  border-radius: 12px;
  margin-top: 20px;
  font-weight: 500;
}

.status-badge {
  margin-top: 18px;
  font-size: 0.85rem;
  opacity: 0.9;
}
</style>