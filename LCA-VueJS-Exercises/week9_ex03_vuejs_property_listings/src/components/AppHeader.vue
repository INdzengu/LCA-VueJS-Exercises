<template>
  <header class="hero-header">
    <div class="hero-overlay">
      <div class="hero-content">
        <p class="hero-subtitle">CAPE TOWN • SOUTH AFRICA</p>
        <h1 class="hero-title">Stay where the mountain<br />meets the sea.</h1>
        <p class="hero-description">
          A small, hand-picked collection of short-term homes across the Cape —
          from Clifton bungalows to Bo-Kaap cottages.
        </p>
      </div>
    </div>

    <div class="controls-container">
      <div class="search-wrapper">
        <span class="search-icon">🔍</span>
        <input
          :value="searchQuery"
          @input="$emit('update:searchQuery', $event.target.value)"
          type="text"
          placeholder="Search by title or location — try 'Clifton' or 'Penthouse'"
          class="search-input"
        />
      </div>

      <button @click="toggleSortOrder" class="sort-btn">
        <span class="sort-icon">⇅</span> Price: {{ currentSortLabel }}
      </button>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
  props: {
    // Stores text entered by user into the search block
    searchQuery: {
      type: String,
      default: "",
    },
    // Remembers current active sort state rule
    sortBy: {
      type: String,
      default: "default",
    },
  },
  computed: {
    // Computes text title printed inside button depending on sorting state
    currentSortLabel() {
      if (this.sortBy === "low-high") return "Low → High";
      if (this.sortBy === "high-low") return "High → Low";
      return "Default";
    },
  },
  methods: {
    // Cycles through sorting states and signals change back to main app file
    toggleSortOrder() {
      let nextSort = "default";
      if (this.sortBy === "default") {
        nextSort = "low-high";
      } else if (this.sortBy === "low-high") {
        nextSort = "high-low";
      } else {
        nextSort = "default";
      }
      this.$emit("update:sortBy", nextSort);
    },
  },
};
</script>

<style scoped>
/* Base configuration layout wrapper for hero header elements */
.hero-header {
  position: relative;
  width: 100%;
  margin-bottom: 80px;
  font-family: sans-serif;
}

/* Background layout applying image overlays and padding properties */
.hero-overlay {
  background:
    linear-gradient(rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.45)),
    url("../assets/background.jpg") no-repeat center center;
  background-size: cover;
  padding: 80px 40px 120px 40px;
  text-align: left;
  color: white;
}

/* Centers header contents safely within maximum layout widths */
.hero-content {
  max-width: 800px;
  margin: 0 auto;
}

/* Tiny subtitle design resting above main heading */
.hero-subtitle {
  font-size: 0.9rem;
  letter-spacing: 3px;
  font-weight: 600;
  margin-bottom: 10px;
  color: whitesmoke;
}

/* Big custom headline styled in white color */
.hero-title {
  font-size: 3rem;
  font-family: "Georgia", serif;
  line-height: 1.2;
  margin: 0 0 15px 0;
  font-weight: 400;
  color: white;
}

/* Soft description paragraph layout text color */
.hero-description {
  font-size: 1.1rem;
  line-height: 1.5;
  max-width: 600px;
  color: lightgrey;
}

/* Overlaying clean box wrapper container holding search inputs */
.controls-container {
  position: absolute;
  bottom: -60px;
  left: 50%;
  transform: translateX(-50%);
  width: 90%;
  max-width: 700px;
  background: white;
  padding: 20px;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  gap: 12px;
  box-sizing: border-box;
  z-index: 10;
}

/* Internal positioning alignment elements wrapper for search bar */
.search-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

/* Magnifying glass position layouts inside search bar */
.search-icon {
  position: absolute;
  left: 15px;
  color: slategrey;
}

/* Warm tone color filled search box fields */
.search-input {
  width: 100%;
  padding: 14px 15px 14px 45px;
  border: none;
  background-color: linen;
  border-radius: 12px;
  font-size: 1rem;
  color: saddlebrown;
  outline: none;
  box-sizing: border-box;
}

/* Soft text placeholder inside input fields color alignment */
.search-input::placeholder {
  color: sienna;
}

/* Core interactive sort filter toggle button styling */
.sort-btn {
  width: 100%;
  padding: 14px;
  background-color: chocolate;
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
  transition: background 0.2s ease;
}

/* Interactive hover response adjustments for button triggers */
.sort-btn:hover {
  background-color: sienna;
}

/* Sorting display character indicators symbol sizing */
.sort-icon {
  font-size: 1.1rem;
}
</style>
