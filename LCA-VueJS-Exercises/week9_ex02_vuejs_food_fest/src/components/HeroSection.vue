<template>
  <header class="hero-section">
    <div class="hero-content">
      <h1>EAT. DRINK.<br /><span class="highlight">CAPE TOWN.</span></h1>
      <p>Three days of fire-cooked food and street markets.</p>
      <div class="hero-buttons">
        <button class="btn-primary" @click="scrollToTickets">
          Grab tickets
        </button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  methods: {
    // DOM manipulation method to improve user navigation
    scrollToTickets() {
      const element = document.querySelector(".ticket-container");
      if (element) {
        element.scrollIntoView({ behavior: "smooth" });
      }
    },
  },
};
</script>
