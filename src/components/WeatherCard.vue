<template>
  <div class="weather-card">
    <h2 class="city-name">{{ cityName }}</h2>

    <div class="icon">{{ weather.icon }}</div>

    <p class="temperature">{{ weather.temperature }}°C</p>
    <p class="condition">{{ weather.condition }}</p>

    <div class="details">
      <div class="detail-item">
        <span class="label">💧 Humidity</span>
        <span class="value">{{ weather.humidity }}%</span>
      </div>
      <div class="detail-item">
        <span class="label">💨 Wind Speed</span>
        <span class="value">{{ weather.windSpeed }} km/h</span>
      </div>
      <div class="detail-item">
        <span class="label">🌡️ Temperature</span>
        <span class="value">{{ weather.temperature }}°C</span>
      </div>
      <div class="detail-item">
        <span class="label">🌥️ Condition</span>
        <span class="value">{{ weather.condition }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WeatherCard',
  props: {
    weather: { type: Object, required: true },
    cityName: { type: String, required: true }
  }
}
</script>

<style scoped>
.weather-card {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  padding: 25px 20px;
  animation: fadeIn 0.5s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(15px); }
  to   { opacity: 1; transform: translateY(0); }
}

.city-name {
  font-size: 1.4rem;
  margin-bottom: 10px;
}

.icon {
  font-size: 4rem;
  margin: 10px 0;
}

.temperature {
  font-size: 3rem;
  font-weight: 700;
}

.condition {
  font-size: 1.1rem;
  margin-bottom: 20px;
  opacity: 0.95;
}

.details {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin-top: 15px;
}

.detail-item {
  background: rgba(255, 255, 255, 0.15);
  padding: 12px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.label {
  font-size: 0.8rem;
  opacity: 0.85;
}

.value {
  font-size: 1rem;
  font-weight: 600;
}
</style>