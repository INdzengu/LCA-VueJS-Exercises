<template>
  <div class="card">
    <h3>{{ title }}</h3>
    <p class="chef-name">Chef: {{ chef }}</p>
    <p class="price-tag">{{ formattedPrice }}</p>
    <p class="level-badge">Level: <strong>{{ level }}</strong></p>
    
    <p v-if="slotsLeft > 0" class="slots-count">🔥 Only {{ slotsLeft }} spots left!</p>
    <p v-else class="sold-out">❌ Sold Out</p>

    <button 
      :disabled="slotsLeft === 0" 
      :class="{ 'btn-disabled': slotsLeft === 0 }"
      @click="$emit('add-to-cart')"
    >
      {{ slotsLeft > 0 ? '🛒 Add to Cart' : 'Unavailable' }}
    </button>
  </div>
</template>

<script>
export default {
  props: ["title", "chef", "price", "level", "slotsLeft"],
  computed: {
    formattedPrice() {
      return `R${this.price.toFixed(2)}`;
    }
  }
};
</script>

<style scoped>
.card {
  border: none;
  padding: 20px;
  width: 240px;
  border-radius: 15px;
  background: white;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.05);
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 22px rgba(0, 0, 0, 0.1);
}
.chef-name { color: #666; font-size: 0.9rem; margin: 5px 0; }
.price-tag { font-size: 1.2rem; font-weight: bold; color: slateblue; margin: 10px 0; }
.level-badge { font-size: 0.85rem; margin-bottom: 10px; }
.slots-count { font-size: 0.8rem; color: #e67e22; font-weight: bold; margin-bottom: 10px; }
.sold-out { color: firebrick; background: #fde8e8; padding: 4px; border-radius: 5px; font-size: 0.8rem; font-weight: bold; margin-bottom: 10px; }

button {
  background: linear-gradient(135deg, #8e44ad, #2c3e50);
  color: white;
  border: none;
  padding: 10px;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  width: 100%;
  transition: background 0.2s;
}
button:hover:not(:disabled) {
  opacity: 0.9;
}
.btn-disabled {
  background: #bdc3c7 !important;
  cursor: not-allowed;
}
</style>