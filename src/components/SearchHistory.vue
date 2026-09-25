<template>
  <div v-if="history.length" class="history">
    <h3 class="history-title">🕘 Recent searches</h3>

    <ul class="history-list">
      <li
        v-for="(item, index) in history"
        :key="item + index"
        class="history-item"
        @click="$emit('select', item)"
        :title="`Search weather for ${item}`"
      >
        <span class="city">{{ item }}</span>
        <button
          class="remove-btn"
          @click.stop="$emit('remove', item)"
          title="Remove from history"
        >
          ✕
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'SearchHistory',
  props: {
    history: {
      type: Array,
      default: () => []
    }
  },
  emits: ['select', 'remove']
}
</script>

<style scoped>
.history {
  margin-top: 20px;
  animation: fadeIn 0.4s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

.history-title {
  font-size: 0.95rem;
  font-weight: 600;
  margin-bottom: 10px;
  opacity: 0.9;
  text-align: left;
}

.history-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  list-style: none;
  padding: 0;
}

.history-item {
  display: flex;
  align-items: center;
  gap: 6px;
  background: rgba(255, 255, 255, 0.2);
  padding: 7px 10px 7px 14px;
  border-radius: 20px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background 0.2s, transform 0.15s;
}

.history-item:hover {
  background: rgba(255, 255, 255, 0.35);
  transform: translateY(-2px);
}

.city {
  font-weight: 500;
}

.remove-btn {
  background: transparent;
  border: none;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.75rem;
  cursor: pointer;
  padding: 2px 5px;
  border-radius: 50%;
  transition: background 0.2s, color 0.2s;
}

.remove-btn:hover {
  background: rgba(255, 80, 80, 0.6);
  color: white;
}
</style>