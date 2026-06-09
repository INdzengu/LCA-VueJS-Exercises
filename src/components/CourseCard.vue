<template>
  <div class="card">
    <h3>{{ title }}</h3>
    <p>Chef: {{ chef }}</p>
    <p>Price: {{ formattedPrice }}</p>
    <p>
      Level: <strong>{{ level }}</strong>
    </p>

    <button v-if="isAvailable" @click="$emit('add-to-wishlist')">
      Save Course
    </button>
    <p v-else class="sold-out">Sold Out</p>
  </div>
</template>

<script>
export default {
  props: ["title", "chef", "price", "level", "isAvailable"],
  computed: {
    // Keeps price display consistent (e.g., R500.00)
    formattedPrice() {
      return `R${this.price.toFixed(2)}`;
    },
  },
};
</script>

<style scoped>
.card {
  border: none;
  padding: 25px;
  width: 280px;
  border-radius: 20px;
  background: white;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.05);
  text-align: center;
  transition:
    transform 0.3s,
    box-shadow 0.3s;
}

/* Hover effect with smooth scaling for interactivity */
.card:hover {
  transform: translateY(-10px) scale(1.02);
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.1);
}

h3 {
  color: darkslategrey;
  margin-bottom: 15px;
}

.sold-out {
  color: firebrick;
  background: lightyellow;
  padding: 5px;
  border-radius: 5px;
  text-transform: uppercase;
  font-size: 0.8rem;
}

button {
  background: linear-gradient(135deg, mediumpurple, slateblue);
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 10px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  filter: brightness(1.1);
}
</style>
