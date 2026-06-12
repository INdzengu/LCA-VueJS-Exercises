<template>
  <div class="app-wrapper">
    <header class="navbar">
      <div class="logo">
        <span class="icon">💪</span>
        <div>
          <h1>FlexZone Fitness</h1>
          <small>STRONGER TOGETHER</small>
        </div>
      </div>
      <div class="nav-right">
        📅 Manage. Schedule. Inspire.
      </div>
    </header>

    <main class="dashboard-container">
      
      <div class="title-bar">
        <div>
          <h2>Class Schedule</h2>
          <p>Add and manage fitness classes for our members.</p>
        </div>
        <div class="counter-box">
          <span class="calendar-icon">📅</span>
          <div>
            <span class="counter-label">Total Sessions</span>
            <span class="counter-value">{{ totalSessions }}</span>
          </div>
        </div>
      </div>

      <div class="grid-layout">
        <section class="sidebar-card">
          <h3 class="section-title"><span class="plus-icon">+</span> Add New Session</h3>
          
          <form @submit.prevent="handleCreateSession">
            <div class="form-group">
              <label>Class Name <span class="required">*</span></label>
              <input type="text" v-model="form.name" placeholder="e.g. HIIT 45">
            </div>

            <div class="form-group">
              <label>Coach / Instructor <span class="required">*</span></label>
              <input type="text" v-model="form.coach" placeholder="e.g. Sarah Johnson">
            </div>

            <div class="form-group">
              <label>Date <span class="required">*</span></label>
              <input type="date" v-model="form.date">
            </div>

            <div class="form-group">
              <label>Time <span class="required">*</span></label>
              <input type="time" v-model="form.time">
            </div>

            <div class="form-group">
              <label>Capacity <span class="required">*</span></label>
              <input type="number" v-model.number="form.capacity" placeholder="e.g. 20">
            </div>

            <button type="submit" class="btn-submit">📅 Add Session</button>
          </form>
          
          <p v-if="errorAlert" class="alert-text">{{ errorAlert }}</p>
          <p class="footer-note">All fields are required.</p>
        </section>

        <section class="main-content-list">
          
          <div class="search-container">
            <label for="search" class="search-label">🔍 Quick Filter Schedule</label>
            <input 
              id="search"
              type="text" 
              v-model="searchQuery" 
              placeholder="Search by class name or coach name..." 
              class="search-input"
            >
          </div>

          <div class="list-title-bar">
            <h3 class="section-title">📋 Scheduled Sessions</h3>
            <span v-if="searchQuery" class="filter-indicator">
              Found: {{ filteredSessions.length }} matching
            </span>
          </div>

          <div v-if="filteredSessions.length > 0" class="sessions-stack">
            <div v-for="(item, index) in filteredSessions" :key="index" class="session-item-card">
              <div class="card-left">
                <div class="avatar-circle">🧘</div>
                <div class="item-details">
                  <h4>{{ item.name }}</h4>
                  <p class="meta-text">👤 <strong>Coach:</strong> {{ item.coach }}</p>
                  <p class="meta-text">📅 <strong>When:</strong> {{ item.date }} &nbsp; 🕒 {{ item.time }}</p>
                </div>
              </div>
              <div class="card-right">
                <span class="spots-indicator">{{ item.capacity }} / {{ item.capacity }} spots</span>
                <button @click="removeSession(index)" class="btn-delete">🗑️ Delete</button>
              </div>
            </div>
          </div>

          <div v-else class="empty-state-card">
            <div class="empty-icon">🔍</div>
            <h4>No matching sessions found.</h4>
            <p>Try searching for a different class name or coach instructor profile.</p>
          </div>
        </section>
      </div>
    </main>

    <footer class="app-footer">
      💜 © 2026 FlexZone Fitness. All rights reserved.
    </footer>
  </div>
</template>

<script>
/**
 * LCA Frontend Web Development: Vue.js Assignment Exercise 04
 * Project Component: Fitness Class Scheduler (Lite Prototype)
 * Target Submission Folder: week9_ex04_vuejs_fitness_scheduler/
 */
export default {
  name: 'App',
  data() {
    return {
      searchQuery: '',
      form: {
        name: '',
        coach: '',
        date: '',
        time: '',
        capacity: null
      },
      errorAlert: '',
      sessions: [
        { name: 'Yoga Flow', coach: 'Sarah Johnson', date: '2026-05-20', time: '09:00', capacity: 20 },
        { name: 'Strength Training', coach: 'Mike Davis', date: '2026-05-20', time: '12:00', capacity: 15 },
        { name: 'Khayelitsha Spinning', coach: 'Sipho', date: '2026-05-21', time: '08:00', capacity: 18 }
      ]
    }
  },
  computed: {
    // Technical Criteria Validation: Tracks current aggregate total instances internally
    totalSessions() {
      return this.sessions.length;
    },
    // Stretch Goal Implementation: Sub-string query evaluation targeting name and instructor values
    filteredSessions() {
      const query = this.searchQuery.toLowerCase().trim();
      
      if (!query) {
        return this.sessions;
      }
      
      return this.sessions.filter(session => {
        return session.name.toLowerCase().includes(query) || 
               session.coach.toLowerCase().includes(query);
      });
    }
  },
  methods: {
    // Core Engine Validation & Creation Control Hook
    handleCreateSession() {
      const coachNamePattern = /^[A-Za-z\s]+$/;

      if (
        !this.form.name.trim() || 
        !this.form.coach.trim() || 
        !this.form.date || 
        !this.form.time || 
        !this.form.capacity
      ) {
        this.errorAlert = "❌ Scheme complete! Please fill in all required fields.";
        return;
      }

      if (!coachNamePattern.test(this.form.coach)) {
        this.errorAlert = "❌ Coach names can only contain letters and spaces (e.g., Coach Thando).";
        return;
      }
      
      if (this.form.capacity <= 0) {
        this.errorAlert = "❌ Capacity must be a positive number greater than 0!";
        return;
      }

      this.errorAlert = '';

      this.sessions.push({ ...this.form });

      this.form.name = '';
      this.form.coach = '';
      this.form.date = '';
      this.form.time = '';
      this.form.capacity = null;
    },
    // Mutation Event: Direct index extraction targeting safe internal removal handling
    removeSession(targetIndex) {
      const confirmCancel = confirm("Are you sure you want to cancel this fitness class?");
      
      if (confirmCancel) {
        const itemToDelete = this.filteredSessions[targetIndex];
        const actualIndex = this.sessions.indexOf(itemToDelete);
        
        if (actualIndex !== -1) {
          this.sessions.splice(actualIndex, 1);
        }
      }
    }
  }
}
</script>

<style scoped>
.app-wrapper {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f8f9fa;
  min-height: 100vh;
  color: #2b2d42;
}
.navbar {
  background-color: #0f1026;
  color: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 40px;
}
.logo {
  display: flex;
  align-items: center;
  gap: 12px;
}
.logo h1 {
  font-size: 1.2rem;
  margin: 0;
  color:white;
}
.logo small {
  color: #a2a3bb;
  letter-spacing: 1px;
}
.dashboard-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 30px 20px;
}
.title-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}
.title-bar h2 {
  font-size: 2rem;
  margin: 0 0 5px 0;
}
.title-bar p {
  color: #6c757d;
  margin: 0;
}
.counter-box {
  background: #f1effb;
  border-radius: 12px;
  padding: 15px 25px;
  display: flex;
  align-items: center;
  gap: 15px;
}
.counter-label {
  display: block;
  font-size: 0.8rem;
  color: #5a52a5;
  font-weight: 600;
}
.counter-value {
  font-size: 1.8rem;
  font-weight: bold;
  color: #4338ca;
}
.grid-layout {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 25px;
}
@media (max-width: 768px) {
  .grid-layout {
    grid-template-columns: 1fr;
  }
}
.sidebar-card, .main-content-list {
  background: #ffffff;
  border-radius: 12px;
  padding: 25px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.02);
}
.section-title {
  font-size: 1.1rem;
  margin-top: 0;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.search-container {
  background-color: #f8f9fa;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 25px;
  border: 1px solid #e9ecef;
}
.search-label {
  display: block;
  font-size: 0.85rem;
  font-weight: bold;
  color: #4f46e5;
  margin-bottom: 8px;
}
.search-input {
  width: 100%;
  padding: 12px;
  border: 2px solid #e2e8f0;
  border-radius: 6px;
  box-sizing: border-box;
  font-size: 0.95rem;
  outline: none;
  transition: border-color 0.2s;
}
.search-input:focus {
  border-color: #6366f1;
}
.list-title-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.filter-indicator {
  font-size: 0.8rem;
  background-color: #fef3c7;
  color: #d97706;
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: 600;
}

.form-group {
  margin-bottom: 15px;
}
.form-group label {
  display: block;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 6px;
}
.required {
  color: #dc3545;
}
.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 6px;
  box-sizing: border-box;
}
.btn-submit {
  width: 100%;
  background-color: #6366f1;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  margin-top: 10px;
}
.btn-submit:hover {
  background-color: #4f46e5;
}
.alert-text {
  color: #dc3545;
  font-size: 0.85rem;
  margin-top: 12px;
  font-weight: bold;
}
.footer-note {
  font-size: 0.75rem;
  color: #a2a3bb;
  text-align: center;
  margin-top: 15px;
}
.sessions-stack {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
.session-item-card {
  border: 1px solid #e9ecef;
  border-radius: 10px;
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.card-left {
  display: flex;
  align-items: center;
  gap: 15px;
}
.avatar-circle {
  width: 45px;
  height: 45px;
  background-color: #f1effb;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
}
.item-details h4 {
  margin: 0 0 5px 0;
  font-size: 1.05rem;
}
.meta-text {
  margin: 0;
  font-size: 0.85rem;
  color: #6c757d;
}
.card-right {
  display: flex;
  align-items: center;
  gap: 20px;
}
.spots-indicator {
  background-color: #e0f2fe;
  color: #0369a1;
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
}
.btn-delete {
  background-color: #fff5f5;
  color: #e53e3e;
  border: 1px solid #fed7d7;
  padding: 8px 14px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.85rem;
}
.btn-delete:hover {
  background-color: #fff0f0;
}
.empty-state-card {
  border: 2px dashed #e9ecef;
  border-radius: 12px;
  padding: 40px;
  text-align: center;
  color: #6c757d;
}
.empty-icon {
  font-size: 2rem;
  margin-bottom: 10px;
}
.app-footer {
  text-align: center;
  padding: 30px;
  color: lightslategrey;
  font-size: 0.85rem;
}
</style>