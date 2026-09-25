<template>
  <div class="loading">
    <div class="spinner"></div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
export default {
  name: 'LoadingMessage',
  props: {
    message: { type: String, default: 'Loading...' }
  }
}
</script>

<style scoped>
.loading {
  padding: 40px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
}

.spinner {
  width: 45px;
  height: 45px;
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top-color: #ffb703;
  border-radius: 50%;
  animation: spin 0.9s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

p {
  font-size: 1rem;
  opacity: 0.95;
}
</style>