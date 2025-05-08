<script setup>
// Props
const props = defineProps({
  showOnlyUncompleted: {
    type: Boolean,
    default: false
  }
});

// Emits
const emit = defineEmits(['update-filter']);

// Methods
const toggleFilter = () => {
  emit('update-filter', !props.showOnlyUncompleted);
};
</script>

<template>
  <div class="filter-controls">
    <div class="filter-option">
      <input 
        type="checkbox" 
        id="show-uncompleted" 
        :checked="showOnlyUncompleted" 
        @change="toggleFilter" 
        class="filter-checkbox"
      />
      <label for="show-uncompleted" class="filter-label">
        <span class="checkbox-custom"></span>
        Show only uncompleted activities
      </label>
    </div>
  </div>
</template>

<style scoped>
.filter-controls {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin-bottom: var(--space-4);
}

.filter-option {
  display: flex;
  align-items: center;
  position: relative;
}

.filter-checkbox {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.filter-label {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: 0.875rem;
  color: var(--gray-700);
}

.checkbox-custom {
  height: 18px;
  width: 18px;
  background-color: white;
  border-radius: 4px;
  margin-right: var(--space-2);
  border: 2px solid var(--primary-400);
  display: inline-block;
  position: relative;
  transition: all 0.2s;
}

.checkbox-custom::after {
  content: "";
  position: absolute;
  display: none;
  left: 5px;
  top: 2px;
  width: 4px;
  height: 8px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.filter-checkbox:checked ~ .filter-label .checkbox-custom {
  background-color: var(--primary-600);
  border-color: var(--primary-600);
}

.filter-checkbox:checked ~ .filter-label .checkbox-custom::after {
  display: block;
}
</style>