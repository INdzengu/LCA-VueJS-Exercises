<template>
  <div class="ticket-card" :class="{ featured: ticket.isFeatured }">
    <h3>{{ ticket.name }}</h3>
    <p class="price">R{{ ticket.price }}</p>
    <ul>
      <li v-for="benefit in ticket.benefits" :key="benefit">{{ benefit }}</li>
    </ul>
    <button @click="$emit('toggle-favourite')">
      {{ ticket.isFavourite ? "★ Favourited" : "☆ Favourite" }}
    </button>
  </div>
</template>

<script>
export default {
  // Receiving data from the parent via props
  props: ["ticket"],
};
</script>

<style scoped>
/* CSS scoped to this component to prevent style leakage */
.ticket-card {
  border: 1px solid #444;
  padding: 20px;
  border-radius: 8px;
  width: 250px;
  background: #2a2220;
  transition: transform 0.3s ease; /* Animation for interaction */
}
.featured {
  border: 2px solid #e8743a;
}
.ticket-card:hover {
  transform: scale(1.05); /* Interactive hover state */
}
</style>
