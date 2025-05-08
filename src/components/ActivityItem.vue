<script setup>
import { ref } from 'vue';

// Props
const props = defineProps({
  activity: {
    type: Object,
    required: true
  }
});

// Emits
const emit = defineEmits(['toggle-complete', 'delete-activity']);

// State
const isDeleting = ref(false);
const confirmDelete = ref(false);

// Methods
const toggleComplete = () => {
  emit('toggle-complete', props.activity.id);
};

const startDelete = () => {
  confirmDelete.value = true;
};

const cancelDelete = () => {
  confirmDelete.value = false;
};

const confirmDeleteActivity = () => {
  isDeleting.value = true;
  emit('delete-activity', props.activity.id);
};

// Format date
const formatDate = (dateString) => {
  const date = new Date(dateString);
  return date.toLocaleDateString('en-US', {
    month: 'short',
    day: 'numeric',
    year: 'numeric'
  });
};
</script>

<template>
  <li class="activity-item" :class="{ 'completed': activity.completed, 'deleting': isDeleting }">
    <div class="activity-content">
      <div class="checkbox-container">
        <input 
          type="checkbox" 
          :checked="activity.completed" 
          @change="toggleComplete" 
          :id="`activity-${activity.id}`" 
          class="checkbox"
        />
        <label :for="`activity-${activity.id}`" class="checkbox-label"></label>
      </div>
      
      <div class="activity-details">
        <p class="activity-text">{{ activity.text }}</p>
        <small class="activity-date">Created on {{ formatDate(activity.createdAt) }}</small>
      </div>
    </div>
    
    <div class="activity-actions" v-if="!confirmDelete">
      <button 
        @click="startDelete" 
        class="delete-button"
        aria-label="Delete activity"
      >
        ✕
      </button>
    </div>
    
    <div class="delete-confirmation" v-else>
      <button @click="confirmDeleteActivity" class="confirm-button">Confirm</button>
      <button @click="cancelDelete" class="cancel-button">Cancel</button>
    </div>
  </li>
</template>

<style scoped>
.activity-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-4);
  border-radius: 0.375rem;
  background-color: var(--gray-50);
  margin-bottom: var(--space-3);
  transition: background-color 0.2s, transform 0.2s, opacity 0.3s;
}

.activity-item:hover {
  background-color: var(--gray-100);
}

.activity-item.completed {
  background-color: var(--gray-100);
  border-left: 3px solid var(--success-500);
}

.activity-item.deleting {
  opacity: 0;
  transform: translateX(-20px);
}

.activity-content {
  display: flex;
  align-items: center;
  flex: 1;
}

.checkbox-container {
  margin-right: var(--space-3);
}

.checkbox {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkbox-label {
  display: inline-block;
  width: 22px;
  height: 22px;
  border-radius: 4px;
  border: 2px solid var(--primary-400);
  background-color: white;
  position: relative;
  cursor: pointer;
  transition: all 0.2s;
}

.checkbox-label::after {
  content: "";
  position: absolute;
  display: none;
  left: 7px;
  top: 3px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.checkbox:checked + .checkbox-label {
  background-color: var(--primary-600);
  border-color: var(--primary-600);
}

.checkbox:checked + .checkbox-label::after {
  display: block;
}

.activity-details {
  flex: 1;
}

.activity-text {
  margin: 0 0 var(--space-1);
  font-size: 1rem;
  transition: text-decoration 0.3s;
}

.completed .activity-text {
  text-decoration: line-through;
  color: var(--gray-500);
}

.activity-date {
  color: var(--gray-500);
  font-size: 0.75rem;
}

.activity-actions {
  display: flex;
  align-items: center;
}

.delete-button {
  background: none;
  border: none;
  color: var(--gray-400);
  font-size: 1rem;
  padding: var(--space-1);
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.delete-button:hover {
  background-color: var(--error-100);
  color: var(--error-600);
}

.delete-confirmation {
  display: flex;
  gap: var(--space-2);
}

.confirm-button {
  background-color: var(--error-500);
  color: white;
  border: none;
  font-size: 0.75rem;
  padding: var(--space-1) var(--space-2);
}

.confirm-button:hover {
  background-color: var(--error-600);
}

.cancel-button {
  background-color: var(--gray-300);
  color: var(--gray-700);
  border: none;
  font-size: 0.75rem;
  padding: var(--space-1) var(--space-2);
}

.cancel-button:hover {
  background-color: var(--gray-400);
}
</style>