<template>
  <div class="container">
    <header class="header">
      <h1>Cooking Masterclass Catalogue</h1>
      <p class="wishlist-counter">Wishlist: {{ wishlistCount }}</p>
    </header>

    <div class="filter-section">
      <button @click="showOnlyAvailable = !showOnlyAvailable">
        {{ showOnlyAvailable ? "Show All Courses" : "Show Available Only" }}
      </button>
    </div>

    <div class="catalogue-grid">
      <CourseCard
        v-for="course in filteredCourses"
        :key="course.id"
        :title="course.title"
        :chef="course.chef"
        :price="course.price"
        :level="course.level"
        :isAvailable="course.isAvailable"
        @add-to-wishlist="wishlistCount++"
      />
    </div>
  </div>
</template>

<script>
import CourseCard from "./components/CourseCard.vue";

export default {
  components: { CourseCard },
  data() {
    return {
      wishlistCount: 0,
      showOnlyAvailable: false,
      courses: [
        {
          id: 1,
          title: "Mastering Italian Pasta",
          chef: "Chef Mario",
          price: 500,
          level: "Beginner",
          isAvailable: false,
        },
        {
          id: 2,
          title: "French Pastry Arts",
          chef: "Chef Sophie",
          price: 850,
          level: "Advanced",
          isAvailable: true,
        },
        {
          id: 3,
          title: "Sushi Rolling 101",
          chef: "Chef Kenji",
          price: 650,
          level: "Intermediate",
          isAvailable: false,
        },
        {
          id: 4,
          title: "Art of Sourdough",
          chef: "Chef Elena",
          price: 450,
          level: "Beginner",
          isAvailable: true,
        },
        {
          id: 5,
          title: "Thai Street Food",
          chef: "Chef Somchai",
          price: 550,
          level: "Intermediate",
          isAvailable: true,
        },
        {
          id: 6,
          title: "Steak Grilling Techniques",
          chef: "Chef Gordon",
          price: 900,
          level: "Advanced",
          isAvailable: false,
        },
        {
          id: 7,
          title: "Vegetarian Delights",
          chef: "Chef Sarah",
          price: 400,
          level: "Beginner",
          isAvailable: false,
        },
        {
          id: 8,
          title: "Mexican Salsa Workshop",
          chef: "Chef Diego",
          price: 350,
          level: "Beginner",
          isAvailable: false,
        },
        {
          id: 9,
          title: "Decadent Chocolate Tempering",
          chef: "Chef Claire",
          price: 750,
          level: "Advanced",
          isAvailable: true,
        },
        {
          id: 10,
          title: "Indian Spice Blending",
          chef: "Chef Arjun",
          price: 500,
          level: "Intermediate",
          isAvailable: false,
        },
        {
          id: 11,
          title: "Mediterranean Seafood",
          chef: "Chef Marco",
          price: 950,
          level: "Advanced",
          isAvailable: true,
        },
        {
          id: 12,
          title: "Quick Weeknight Meals",
          chef: "Chef Jamie",
          price: 300,
          level: "Beginner",
          isAvailable: true,
        },
      ],
    };
  },
  computed: {
    // Dynamic filter: updates the list whenever the user clicks the toggle
    filteredCourses() {
      return this.showOnlyAvailable
        ? this.courses.filter((c) => c.isAvailable)
        : this.courses;
    },
  },
};
</script>

<style>
.container {
  font-family: sans-serif;
  padding: 20px;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 20px;
  border-bottom: 2px solid #eee;
  margin-bottom: 20px;
}
.wishlist-counter {
  font-weight: bold;
  font-size: 1.2rem;
  background: #eee;
  padding: 10px;
  border-radius: 10px;
}
.filter-section {
  text-align: center;
  margin-bottom: 30px;
}
.catalogue-grid {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
}
.filter-section button {
  background-color: transparent;
  color: slateblue;
  border: 2px solid slateblue;
  padding: 12px 25px;
  border-radius: 30px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-bottom: 20px;
}

.filter-section button:hover {
  background-color: slateblue;
  color: white;
  transform: translateY(-2px);
}

.filter-section button:active {
  transform: translateY(0);
}
</style>
