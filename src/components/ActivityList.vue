<script setup>
import { TransitionGroup } from 'vue';
import ActivityItem from './ActivityItem.vue';

// Props
const props = defineProps({
  activities: {
    type: Array,
    required: true
  }
});

// Emits
const emit = defineEmits(['toggle-complete', 'delete-activity']);

// Methods
const handleToggleComplete = (id) => {
  emit('toggle-complete', id);
};

const handleDeleteActivity = (id) => {
  emit('delete-activity', id);
};
</script>

<template>
  <section class="activity-list-section">
    <h2>Your Activities</h2>
    
    <div v-if="activities.length === 0" class="empty-state">
      <p>No activities found. Add a new one to get started!</p>
    </div>
    
    <TransitionGroup name="list" tag="ul" class="activity-list" v-else>
      <ActivityItem
        v-for="activity in activities"
        :key="activity.id"
        :activity="activity"
        @toggle-complete="handleToggleComplete"
        @delete-activity="handleDeleteActivity"
      />
    </TransitionGroup>
  </section>
</template>

<style scoped>
.activity-list-section {
  background-color: white;
  border-radius: 0.5rem;
  padding: var(--space-6);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.activity-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.empty-state {
  text-align: center;
  padding: var(--space-8) 0;
  color: var(--gray-500);
}

/* List transitions */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

.list-move {
  transition: transform 0.5s ease;
}
</style>