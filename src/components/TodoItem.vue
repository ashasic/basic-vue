<template>
  <div class="todo-card" :class="{ 'completed': todoItem.completed }">
    <div class="todo-content">
      <div class="todo-main">
        <h5 class="todo-title" :class="{ 'strikethrough': todoItem.completed }">
          {{ todoItem.description }}
        </h5>
        <div class="todo-meta">
          <span class="course-badge">{{ todoItem.course }}</span>
          <span class="due-date">📅 Due: {{ formatDate(todoItem.dueDate) }}</span>
        </div>
      </div>
      <div class="todo-actions">
        <button 
          @click="toggleComplete"
          :class="['action-btn', todoItem.completed ? 'btn-undo' : 'btn-complete']"
        >
          {{ todoItem.completed ? '↩️ Undo' : '✅ Complete' }}
        </button>
        <button @click="removeTodo" class="action-btn btn-remove">
          🗑️ Remove
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  todoItem: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle-complete', 'remove-todo'])

const toggleComplete = () => {
  emit('toggle-complete', props.todoItem.id)
}

const removeTodo = () => {
  emit('remove-todo', props.todoItem.id)
}

const formatDate = (dateString) => {
  const date = new Date(dateString)
  return date.toLocaleDateString('en-US', { 
    year: 'numeric', 
    month: 'short', 
    day: 'numeric' 
  })
}
</script>

<style scoped>
.todo-card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
  border: 2px solid transparent;
  overflow: hidden;
}

.todo-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.12);
  border-color: #ff6b6b;
}

.todo-card.completed {
  background: linear-gradient(135deg, #f8f9fa, #e9ecef);
  opacity: 0.8;
}

.todo-content {
  padding: 1.5rem;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
}

.todo-main {
  flex: 1;
}

.todo-title {
  font-size: 1.2rem;
  font-weight: 600;
  color: #333;
  margin: 0 0 0.8rem 0;
  line-height: 1.4;
  transition: all 0.3s ease;
}

.todo-title.strikethrough {
  text-decoration: line-through;
  color: #6c757d;
}

.todo-meta {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.course-badge {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
  color: white;
  padding: 0.4rem 1rem;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: 600;
  display: inline-block;
  width: fit-content;
  box-shadow: 0 2px 8px rgba(255, 107, 107, 0.3);
}

.due-date {
  font-size: 0.9rem;
  color: #666;
  font-weight: 500;
}

.todo-actions {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex-shrink: 0;
}

.action-btn {
  padding: 0.6rem 1.2rem;
  border: none;
  border-radius: 10px;
  font-weight: 600;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.3s ease;
  white-space: nowrap;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.btn-complete {
  background: linear-gradient(135deg, #51cf66, #37c940);
  color: white;
}

.btn-complete:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(81, 207, 102, 0.4);
}

.btn-undo {
  background: linear-gradient(135deg, #ffd43b, #fab005);
  color: white;
}

.btn-undo:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(255, 212, 59, 0.4);
}

.btn-remove {
  background: linear-gradient(135deg, #ff8787, #ff6b6b);
  color: white;
}

.btn-remove:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(255, 135, 135, 0.4);
}

.action-btn:active {
  transform: translateY(0);
}

@media (max-width: 768px) {
  .todo-content {
    flex-direction: column;
    gap: 1rem;
  }
  
  .todo-actions {
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
  }
  
  .action-btn {
    flex: 1;
    margin: 0 0.25rem;
  }
}
</style>