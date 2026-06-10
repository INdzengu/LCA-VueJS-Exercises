<template>
  <div id="app-container">
    <AppHeader v-model:searchQuery="searchQuery" v-model:sortBy="sortBy" />

    <main class="main-content">
      <div v-if="filteredProperties.length > 0" class="property-grid">
        <PropertyCard
          v-for="item in filteredProperties"
          :key="item.id"
          :property="item"
          @toggle-bookmark="handleBookmark"
        />
      </div>

      <div v-else class="no-results">
        <p>
          No properties match your current search criteria. Try filtering
          another area!
        </p>
      </div>
    </main>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
import PropertyCard from "./components/PropertyCard.vue";

export default {
  name: "App",
  components: {
    AppHeader,
    PropertyCard,
  },
  data() {
    return {
      // Keeps record track of search strings typed inside headers boxes
      searchQuery: "",
      // Remembers active arrangement sorting filter toggles states rules
      sortBy: "default",
      // Master database collection array housing 10 Cape Town property listing records
      properties: [
        {
          id: 1,
          title: "Sea Point Flat",
          location: "Sea Point",
          price: 1200,
          type: "Apartment",
          image: "https://picsum.photos/400/300?random=1",
          beds: 2,
          baths: 1,
          guests: 4,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 2,
          title: "City Bowl Studio",
          location: "Cape Town Central",
          price: 850,
          type: "Studio",
          image: "https://picsum.photos/400/300?random=2",
          beds: 1,
          baths: 1,
          guests: 2,
          isAvailable: false,
          isBookmarked: false,
        },
        {
          id: 3,
          title: "Clifton Villa",
          location: "Clifton",
          price: 3500,
          type: "Villa",
          image: "https://picsum.photos/400/300?random=3",
          beds: 4,
          baths: 4,
          guests: 8,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 4,
          title: "Gardens Cottage",
          location: "Gardens",
          price: 950,
          type: "Cottage",
          image: "https://picsum.photos/400/300?random=4",
          beds: 2,
          baths: 1,
          guests: 3,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 5,
          title: "Green Point Loft",
          location: "Green Point",
          price: 1100,
          type: "Apartment",
          image: "https://picsum.photos/400/300?random=5",
          beds: 1,
          baths: 1,
          guests: 2,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 6,
          title: "Tamboerskloof House",
          location: "Tamboerskloof",
          price: 2800,
          type: "House",
          image: "https://picsum.photos/400/300?random=6",
          beds: 3,
          baths: 2,
          guests: 6,
          isAvailable: false,
          isBookmarked: false,
        },
        {
          id: 7,
          title: "Vredehoek Suite",
          location: "Vredehoek",
          price: 700,
          type: "Apartment",
          image: "https://picsum.photos/400/300?random=7",
          beds: 1,
          baths: 1,
          guests: 2,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 8,
          title: "Camps Bay Retreat",
          location: "Camps Bay",
          price: 4200,
          type: "Villa",
          image: "https://picsum.photos/400/300?random=8",
          beds: 5,
          baths: 5,
          guests: 10,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 9,
          title: "Observatory Share",
          location: "Observatory",
          price: 500,
          type: "Room",
          image: "https://picsum.photos/400/300?random=9",
          beds: 1,
          baths: 1,
          guests: 1,
          isAvailable: true,
          isBookmarked: false,
        },
        {
          id: 10,
          title: "Woodstock Industrial",
          location: "Woodstock",
          price: 1300,
          type: "Studio",
          image: "https://picsum.photos/400/300?random=10",
          beds: 1,
          baths: 1,
          guests: 2,
          isAvailable: true,
          isBookmarked: false,
        },
      ],
    };
  },
  computed: {
    // Dynamic background data processing loop combining keyword checks and sorting rules
    filteredProperties() {
      let result = [...this.properties];

      // 1. Drops records out of sight if they fail matching text entries filters strings
      if (this.searchQuery.trim() !== "") {
        const query = this.searchQuery.toLowerCase();
        result = result.filter(
          (p) =>
            p.title.toLowerCase().includes(query) ||
            p.location.toLowerCase().includes(query),
        );
      }

      // 2. Adjusts matching list items array indexing orders depending on pricing rates parameters
      if (this.sortBy === "low-high") {
        result.sort((a, b) => a.price - b.price);
      } else if (this.sortBy === "high-low") {
        result.sort((a, b) => b.price - a.price);
      }

      return result;
    },
  },
  methods: {
    // Locates child component item inside arrays and toggles boolean bookmark markers safely
    handleBookmark(propertyId) {
      const found = this.properties.find((p) => p.id === propertyId);
      if (found) {
        found.isBookmarked = !found.isBookmarked;
      }
    },
  },
};
</script>

<style>
/* Reset baseline default browser rules margins settings around frame layouts */
body {
  margin: 0;
  padding: 0;
  background-color: oldlace;
}

/* Stretches main app wrapper shell boundary grids fully across screen widths */
#app-container {
  width: 100%;
}

/* Center alignments containment framing for cards layouts grids displays */
.main-content {
  max-width: 1200px;
  margin: 50px auto;
  padding: 0 24px;
  box-sizing: border-box;
}

/* Modern responsive grid system aligning property items rows dynamically across views */
.property-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 30px;
}

/* Fallback print block texts warnings warning layouts styling alignments */
.no-results {
  text-align: center;
  color: sienna;
  margin-top: 60px;
  font-size: 1.1rem;
  font-family: sans-serif;
}
</style>
