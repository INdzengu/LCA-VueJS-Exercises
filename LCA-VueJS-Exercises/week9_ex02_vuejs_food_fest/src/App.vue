<template>
  <div class="food-fest-app">
    <HeroSection />

    <section class="filter-section">
      <label for="priceRange">Filter by Price: R{{ maxPrice }}</label>
      <input
        type="range"
        id="priceRange"
        min="0"
        max="2000"
        step="50"
        v-model="maxPrice"
      />
    </section>

    <div class="ticket-container">
      <TicketCard
        v-for="ticket in filteredTickets"
        :key="ticket.id"
        :ticket="ticket"
        @toggle-favourite="toggleFavourite(ticket.id)"
      />
    </div>
  </div>
</template>

<script>
import TicketCard from "./components/TicketCard.vue";
import HeroSection from "./components/HeroSection.vue";

export default {
  components: { TicketCard, HeroSection },
  data() {
    return {
      maxPrice: 2000,
      // Local state store for all ticket information
      tickets: [
        {
          id: 1,
          name: "Bronze",
          price: 350,
          benefits: ["Access to main stage", "Food stalls"],
          isFeatured: false,
          isFavourite: false,
        },
        {
          id: 2,
          name: "Silver",
          price: 690,
          benefits: ["VIP Lounge access", "Festival gift pack"],
          isFeatured: true,
          isFavourite: false,
        },
        {
          id: 3,
          name: "Gold",
          price: 1490,
          benefits: ["Backstage pass", "Unlimited food"],
          isFeatured: false,
          isFavourite: false,
        },
      ],
    };
  },
  computed: {
    // Computed property: Automatically filters tickets whenever maxPrice changes
    filteredTickets() {
      return this.tickets.filter((ticket) => ticket.price <= this.maxPrice);
    },
  },
  methods: {
    // Method to update parent state based on child interaction
    toggleFavourite(id) {
      const ticket = this.tickets.find((t) => t.id === id);
      if (ticket) {
        ticket.isFavourite = !ticket.isFavourite;
      }
    },
  },
};
</script>
