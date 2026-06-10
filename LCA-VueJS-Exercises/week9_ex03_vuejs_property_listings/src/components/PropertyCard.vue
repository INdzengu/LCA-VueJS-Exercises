<template>
  <div class="card" :class="{ 'unavailable-card': !property.isAvailable }">
    <div class="image-container">
      <img :src="property.image" :alt="property.title" class="property-img" />

      <div class="badge type-badge">{{ property.type }}</div>
      <div v-if="!property.isAvailable" class="badge status-badge">
        Unavailable
      </div>
    </div>

    <div class="content">
      <p class="location">📍 {{ property.location }}</p>
      <h3 class="title">{{ property.title }}</h3>

      <div class="details">
        <span class="spec-item">🛏️ {{ property.beds }} Beds</span>
        <span class="spec-item">🛁 {{ property.baths }} Baths</span>
        <span class="spec-item">👥 {{ property.guests }} Guests</span>
      </div>

      <hr class="card-divider" />

      <div class="footer">
        <p class="price">
          <span class="amount">R{{ property.price.toLocaleString() }}</span>
          <span class="unit"> / NIGHT</span>
        </p>

        <button
          @click="$emit('toggle-bookmark', property.id)"
          class="bookmark-btn"
          :class="{ 'is-active': property.isBookmarked }"
          :aria-label="'Bookmark ' + property.title"
        >
          {{ property.isBookmarked ? "★" : "☆" }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PropertyCard",
  props: {
    // Accepts full listing details structure safely from parent collection data stream
    property: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
/* Main exterior boundaries layout shell container for listings cards */
.card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition:
    transform 0.25s ease,
    box-shadow 0.25s ease;
}

/* Card reactive elevation animation response when mouse pointer hovers over */
.card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
}

/* Applies soft visual translucent fading effect if items are sold out */
.unavailable-card {
  opacity: 0.8;
}

/* Image containment boundaries positioning frame */
.image-container {
  position: relative;
  height: 220px;
  width: 100%;
  background-color: lightgrey;
}

/* Forces image to expand fully and cover dimensions cleanly without skewing proportions */
.property-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Universal structural configuration layout rule adjustments for overlay tags */
.badge {
  position: absolute;
  padding: 6px 14px;
  border-radius: 30px;
  font-size: 0.72rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

/* Floating room layout style type tag block color alignments */
.type-badge {
  top: 14px;
  left: 14px;
  background: rgba(255, 255, 255, 0.95);
  color: saddlebrown;
}

/* High alert red color status flag overlay if room listing is fully booked */
.status-badge {
  top: 14px;
  right: 14px;
  background: crimson;
  color: white;
}

/* Padding dimensions around texts descriptions fields layout blocks */
.content {
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

/* Small highlighted terracotta colored location suburb label tags text */
.location {
  margin: 0;
  font-size: 0.8rem;
  color: chocolate;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.8px;
}

/* Elegant deep tinted dark brown listing heading title typeface */
.title {
  margin: 0 0 4px 0;
  font-family: "Georgia", serif;
  font-size: 1.35rem;
  color: darkslategray;
  font-weight: 400;
}

/* Configuration grid line details for icon markers tracking room components */
.details {
  display: flex;
  gap: 16px;
  font-size: 0.85rem;
  color: dimgray;
  margin: 6px 0;
}

/* Horizontal flex align adjustments across inner row details content */
.spec-item {
  display: flex;
  align-items: center;
  gap: 4px;
}

/* Ultra fine separating color breaking rule bar lines */
.card-divider {
  border: 0;
  height: 1px;
  background: seashell;
  margin: 12px 0;
}

/* Spacing out elements across lower footer border layout boundaries */
.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Reset baseline paragraph typography styles across price elements */
.price {
  margin: 0;
  font-family: sans-serif;
}

/* Big bold numeric digit lettering highlighting price costs rate values */
.amount {
  font-size: 1.3rem;
  font-weight: 700;
  color: darkslategray;
}

/* Tiny tracking sub text label attached adjacent to currency rates numbers */
.unit {
  font-size: 0.78rem;
  color: slategrey;
  font-weight: 600;
  letter-spacing: 0.5px;
}

/* Circular button styling architecture for tracking favorite clicks items */
.bookmark-btn {
  background: linen;
  border: none;
  font-size: 1.2rem;
  color: slategrey;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.2s ease;
  padding: 0;
}

/* Magnifies circle scales marginally when hovered by mouse pointers controls */
.bookmark-btn:hover {
  background: wheat;
  transform: scale(1.05);
}

/* Distinct color shifting changes filling button solid gold-terracotta when bookmarked active */
.bookmark-btn.is-active {
  background: chocolate;
  color: white;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}
</style>
