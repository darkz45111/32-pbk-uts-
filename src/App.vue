<script setup>
import { ref, computed, onMounted, watch } from 'vue';
import ActivityForm from './components/ActivityForm.vue';
import ActivityList from './components/ActivityList.vue';
import FilterControls from './components/FilterControls.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

// State
const activities = ref([]);
const showOnlyUncompleted = ref(false);

// Filtered activities
const filteredActivities = computed(() => {
  if (showOnlyUncompleted.value) {
    return activities.value.filter(activity => !activity.completed);
  }
  return activities.value;
});

// Load activities from localStorage
onMounted(() => {
  const savedActivities = localStorage.getItem('activities');
  if (savedActivities) {
    activities.value = JSON.parse(savedActivities);
  }
});

// Save activities to localStorage whenever they change
watch(activities, (newActivities) => {
  localStorage.setItem('activities', JSON.stringify(newActivities));
}, { deep: true });

// Add a new activity
const addActivity = (activity) => {
  activities.value.push(activity);
};

// Toggle activity completion status
const toggleComplete = (id) => {
  const activity = activities.value.find(activity => activity.id === id);
  if (activity) {
    activity.completed = !activity.completed;
  }
};

// Delete an activity
const deleteActivity = (id) => {
  activities.value = activities.value.filter(activity => activity.id !== id);
};

// Update filter
const updateFilter = (showUncompleted) => {
  showOnlyUncompleted.value = showUncompleted;
};
</script>

<template>
  <div class="app-container">
    <Header />
    
    <main class="container">
      <ActivityForm @add-activity="addActivity" />
      
      <div class="content-section">
        <FilterControls 
          :show-only-uncompleted="showOnlyUncompleted" 
          @update-filter="updateFilter" 
        />
        
        <ActivityList 
          :activities="filteredActivities" 
          @toggle-complete="toggleComplete" 
          @delete-activity="deleteActivity" 
        />
      </div>
    </main>
    
    <Footer />
  </div>
</template>

<style>
.app-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}

.content-section {
  margin-top: var(--space-8);
}
</style>


