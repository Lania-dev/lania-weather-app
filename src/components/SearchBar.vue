<template>
  <div class="search-bar">
    <input
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      @keyup.enter="$emit('search')"
      placeholder="Enter a city name..."
      class="city-input"
    />
    <button @click="$emit('search')" :disabled="loading" class="search-btn">
      <span v-if="!loading">🔍 Search</span>
      <span v-else>⏳...</span>
    </button>
  </div>
</template>

<script>
export default {
  name: 'SearchBar',
  props: {
    modelValue: { type: String, default: '' },
    loading: { type: Boolean, default: false }
  },
  emits: ['update:modelValue', 'search']
}
</script>

<style scoped>
.search-bar {
  display: flex;
  gap: 10px;
  margin-bottom: 25px;
}

.city-input {
  flex: 1;
  padding: 13px 16px;
  border-radius: 10px;
  border: none;
  outline: none;
  font-size: 1rem;
  background: rgba(255, 255, 255, 0.9);
  color: #333;
}

.city-input:focus {
  box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.5);
}

.search-btn {
  padding: 13px 20px;
  border-radius: 10px;
  border: none;
  background: #ffb703;
  color: #1a1a1a;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: transform 0.2s, background 0.2s;
}

.search-btn:hover:not(:disabled) {
  background: #ffca3a;
  transform: translateY(-2px);
}

.search-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
</style>