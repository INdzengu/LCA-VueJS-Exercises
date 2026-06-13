<template>
  <div class="app-wrapper">
    
    <header class="main-header">
      <div class="header-content">
        <div class="logo-area">
          <span class="logo-icon">🍳</span>
          <div>
            <h1>COOKING MASTERCLASS</h1>
            <p class="tagline">Culinary Excellence in Cape Town</p>
          </div>
        </div>
        <div class="header-status">
          <span class="status-badge">Checkout</span>
        </div>
      </div>
    </header>

    <div class="container">
      <div class="checkout-layout">
        
        <section class="catalogue-section">
          <h2>Available Masterclasses</h2>
          <div class="catalogue-grid">
            <CourseCard
              v-for="course in courses"
              :key="course.id"
              :title="course.title"
              :chef="course.chef"
              :price="course.price"
              :level="course.level"
              :slotsLeft="course.slotsLeft"
              @add-to-cart="addToCart(course)"
            />
          </div>
        </section>

        <section class="checkout-panel">
          <h2>🛒 Your Checkout Invoice</h2>
          
          <div v-if="cart.length === 0" class="empty-cart-view">
            <p>Your invoice summary is currently empty.</p>
            <p class="hint">Click "Add to Cart" on the left to start a booking order.</p>
          </div>
          
          <div v-else>
            <div class="cart-items-list">
              <div v-for="item in cart" :key="item.id" class="invoice-row-item">
                <div class="item-details">
                  <h4>{{ item.title }}</h4>
                  <p class="item-rate">R{{ item.price.toFixed(2) }} each</p>
                </div>
                
                <div class="qty-control-box">
                  <button @click="decreaseQty(item)" class="control-btn minus">-</button>
                  <span class="qty-display">{{ item.quantity }}</span>
                  <button @click="increaseQty(item)" class="control-btn plus">+</button>
                </div>
              </div>
            </div>

            <button @click="clearEntireCart" class="btn-clear-all">🗑️ Clear Entire Selection</button>

            <hr class="divider" />

            <div class="financials-ledger">
              <p class="ledger-line">Subtotal: <span>R{{ totals.subtotal.toFixed(2) }}</span></p>
              <p class="ledger-line">VAT Tax (15%): <span>R{{ totals.vat.toFixed(2) }}</span></p>
              <div class="ledger-line grand-total-row">
                <strong>Grand Total:</strong>
                <strong class="total-amount">R{{ totals.grandTotal.toFixed(2) }}</strong>
              </div>
            </div>
            
            <button @click="triggerSimulatedCheckout" class="btn-checkout-confirm">
              ✅ Complete Checkout Simulation
            </button>
          </div>
        </section>

      </div>
    </div>

    <footer class="main-footer">
      <div class="footer-content">
        <p>&copy; 2026 Cooking Masterclass SA. Built for product review demonstration.</p>
        <p class="footer-location">📍 Bree Street, Cape Town | 📞 +27 21 555 1234</p>
      </div>
    </footer>

  </div>
</template>

<script>
import CourseCard from "./components/CourseCard.vue";

export default {
  components: { CourseCard },
  data() {
    return {
      cart: [],
      courses: [
        { id: 1, title: "Mastering Italian Pasta", chef: "Chef Mario", price: 500, level: "Beginner", slotsLeft: 4 },
        { id: 2, title: "French Pastry Arts", chef: "Chef Sophie", price: 850, level: "Advanced", slotsLeft: 0 },
        { id: 3, title: "Sushi Rolling 101", chef: "Chef Kenji", price: 650, level: "Intermediate", slotsLeft: 3 },
        { id: 4, title: "Art of Sourdough", chef: "Chef Elena", price: 450, level: "Beginner", slotsLeft: 6 },
        { id: 5, title: "Thai Street Food", chef: "Chef Somchai", price: 550, level: "Intermediate", slotsLeft: 2 },
        { id: 6, title: "Mexican Street Tacos", chef: "Chef Alejandro", price: 400, level: "Beginner", slotsLeft: 5 },
        { id: 7, title: "Indian Curry Secrets", chef: "Chef Priya", price: 600, level: "Advanced", slotsLeft: 4 },
        { id: 8, title: "Spanish Paella Masterclass", chef: "Chef Carlos", price: 700, level: "Intermediate", slotsLeft: 1 },
        { id: 9, title: "Gourmet Burger Crafting", chef: "Chef Marcus", price: 350, level: "Beginner", slotsLeft: 8 },
        { id: 10, title: "Artisanal Chocolate Making", chef: "Chef Chloe", price: 900, level: "Advanced", slotsLeft: 3 },
        { id: 11, title: "Traditional South African Braai", chef: "Chef Thabo", price: 950, level: "Advanced", slotsLeft: 5 },
        { id: 12, title: "Quick Weeknight Meals", chef: "Chef Jamie", price: 300, level: "Beginner", slotsLeft: 7 }
      ]
    };
  },
  computed: {
    totals() {
      let subtotal = 0;
      this.cart.forEach(item => {
        subtotal += item.price * item.quantity;
      });
      const vat = subtotal * 0.15;
      const grandTotal = subtotal + vat;
      return { subtotal, vat, grandTotal };
    }
  },
  methods: {
    addToCart(course) {
      if (course.slotsLeft <= 0) return;
      const existingItem = this.cart.find(item => item.id === course.id);
      if (existingItem) {
        existingItem.quantity++;
      } else {
        this.cart.push({ id: course.id, title: course.title, price: course.price, quantity: 1 });
      }
      course.slotsLeft--;
    },
    increaseQty(item) {
      const course = this.courses.find(c => c.id === item.id);
      if (course && course.slotsLeft > 0) {
        item.quantity++;
        course.slotsLeft--;
      } else {
        alert("Sorry! No additional slots available for this masterclass.");
      }
    },
    decreaseQty(item) {
      const course = this.courses.find(c => c.id === item.id);
      item.quantity--;
      if (course) {
        course.slotsLeft++;
      }
      if (item.quantity === 0) {
        this.cart = this.cart.filter(c => c.id !== item.id);
      }
    },
    clearEntireCart() {
      this.cart.forEach(item => {
        const course = this.courses.find(c => c.id === item.id);
        if (course) { course.slotsLeft += item.quantity; }
      });
      this.cart = [];
    },
    triggerSimulatedCheckout() {
      alert(`🎉 Checkout successful! Total Billed: R${this.totals.grandTotal.toFixed(2)}. This completes the design system simulation.`);
      this.cart = [];
    }
  }
};
</script>

<style>
/* CSS Global Page Layout Rules Reset */
body { margin: 0; padding: 0; background-color: #f4f6f9; }

.app-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* 1. PREMIUM HERO HEADER STYLING (Matches uploaded image style layout) */
.main-header {
  background: linear-gradient(135deg, #1e1e24 0%, #2a2a35 100%);
  color: #ffffff;
  padding: 20px 40px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}
.logo-area { display: flex; align-items: center; gap: 15px; }
.logo-icon { font-size: 2.5rem; }
.main-header h1 { margin: 0; font-size: 1.6rem; letter-spacing: 1px; color: #f39c12; }
.tagline { margin: 2px 0 0 0; font-size: 0.85rem; color: #bdc3c7; }
.status-badge {
  background-color: rgba(243, 156, 18, 0.2);
  color: #f39c12;
  border: 1px solid #f39c12;
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: bold;
}

/* 2. BODY CONTENT CONTENT FRAME HOUSES */
.container { 
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
  padding: 30px; 
  flex: 1; /* Pushes footer down cleanly */
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
  box-sizing: border-box;
}

.checkout-layout { display: flex; gap: 30px; flex-wrap: wrap; align-items: flex-start; }
.catalogue-section { flex: 2; min-width: 450px; }
.catalogue-section h2, .checkout-panel h2 { font-size: 1.3rem; margin-top: 0; margin-bottom: 20px; border-bottom: 2px solid #ddd; padding-bottom: 10px; }
.catalogue-grid { display: flex; gap: 20px; flex-wrap: wrap; justify-content: flex-start; }

/* Sticky Sidecar Invoice calculations panel style rules */
.checkout-panel { flex: 1; min-width: 320px; background: white; padding: 25px; border-radius: 15px; box-shadow: 0 4px 20px rgba(0,0,0,0.06); position: sticky; top: 20px; }
.empty-cart-view { text-align: center; color: #95a5a6; padding: 40px 10px; }
.empty-cart-view .hint { font-size: 0.85rem; color: #bdc3c7; margin-top: 10px; }
.invoice-row-item { display: flex; justify-content: space-between; align-items: center; padding: 12px 0; border-bottom: 1px dashed #ecf0f1; }
.item-details h4 { margin: 0 0 4px 0; font-size: 0.95rem; }
.item-rate { margin: 0; font-size: 0.85rem; color: #7f8c8d; }

.qty-control-box { display: flex; align-items: center; background: #f8f9fa; padding: 4px 8px; border-radius: 8px; border: 1px solid #e2e8f0; }
.control-btn { border: none; background: #e2e8f0; font-weight: bold; width: 24px; height: 24px; border-radius: 4px; cursor: pointer; }
.qty-display { font-weight: bold; width: 30px; text-align: center; font-size: 0.9rem; }
.btn-clear-all { background: none; border: none; color: #e74c3c; font-size: 0.85rem; cursor: pointer; margin-top: 15px; padding: 0; }
.divider { border: 0; border-top: 1px solid #eaeaea; margin: 20px 0; }
.ledger-line { display: flex; justify-content: space-between; margin: 8px 0; font-size: 0.95rem; }
.grand-total-row { border-top: 2px solid #2c3e50; padding-top: 12px; margin-top: 12px; font-size: 1.15rem; }
.total-amount { color: #8e44ad; }
.btn-checkout-confirm { width: 100%; background: #27ae60; color: white; border: none; padding: 12px; border-radius: 8px; font-weight: bold; font-size: 1rem; cursor: pointer; margin-top: 10px; }

/* 3. BRAND NEW FOOTER DISPLAY STYLING */
.main-footer {
  background-color: #2c3e50;
  color: #ecf0f1;
  text-align: center;
  padding: 20px;
  margin-top: auto;
  font-size: 0.85rem;
}
.footer-content p { margin: 5px 0; }
.footer-location { color: #bdc3c7; font-size: 0.8rem; }

/* Responsive break points code adapters */
@media (max-width: 850px) {
  .checkout-layout { flex-direction: column; }
  .catalogue-section, .checkout-panel { width: 100%; flex: none; }
  .checkout-panel { position: static; }
  .header-content { flex-direction: column; gap: 10px; text-align: center; }
  .logo-area { flex-direction: column; gap: 5px; }
}
</style>