<script setup>
import { ref } from 'vue';
import { v4 as uuidv4 } from 'uuid';

// Props and emits
const emit = defineEmits(['add-activity']);

// State
const activityText = ref('');
const isSubmitting = ref(false);
const errorMessage = ref('');

// Methods
const submitActivity = () => {
  // Validate
  if (!activityText.value.trim()) {
    errorMessage.value = 'Please enter an activity';
    return;
  }
  
  errorMessage.value = '';
  isSubmitting.value = true;
  
  // Create new activity
  const newActivity = {
    id: uuidv4(),
    text: activityText.value.trim(),
    completed: false,
    createdAt: new Date().toISOString()
  };
  
  // Emit event to add the activity
  emit('add-activity', newActivity);
  
  // Reset form
  activityText.value = '';
  isSubmitting.value = false;
};
</script>

<template>
  <section class="activity-form-section">
    <h2>Add New Activity</h2>
    
    <form @submit.prevent="submitActivity" class="activity-form">
      <div class="form-group">
        <label for="activity-text" class="form-label">Activity Description</label>
        <div class="input-container">
          <input
            id="activity-text"
            v-model="activityText"
            type="text"
            placeholder="Enter your activity..."
            class="activity-input"
            :class="{ 'error': errorMessage }"
          />
          <button 
            type="submit" 
            class="add-button"
            :disabled="isSubmitting"
          >
            Add
          </button>
        </div>
        <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      </div>
    </form>
  </section>
</template>

<style scoped>
.activity-form-section {
  background-color: white;
  border-radius: 0.5rem;
  padding: var(--space-6);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  margin-top: var(--space-6);
}

.activity-form {
  margin-top: var(--space-4);
}

.form-group {
  margin-bottom: var(--space-4);
}

.form-label {
  display: block;
  margin-bottom: var(--space-2);
  font-weight: 500;
  color: var(--gray-700);
}

.input-container {
  display: flex;
  gap: var(--space-2);
}

.activity-input {
  flex: 1;
}

.activity-input.error {
  border-color: var(--error-500);
}

.add-button {
  background-color: var(--primary-600);
  color: white;
  border: none;
  font-weight: 500;
  padding: var(--space-2) var(--space-5);
  border-radius: 0.375rem;
  transition: background-color 0.2s;
}

.add-button:hover {
  background-color: var(--primary-700);
}

.add-button:disabled {
  background-color: var(--gray-400);
  cursor: not-allowed;
}

.error-message {
  color: var(--error-500);
  font-size: 0.875rem;
  margin-top: var(--space-2);
  margin-bottom: 0;
}
</style>