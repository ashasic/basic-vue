<script setup>
import { ref, computed } from 'vue'
import TodoItem from './components/TodoItem.vue'

const courses = [
  'Topics in Computer Science II',
  'Intro to Networks',
  'Theory of Computation'
]

const todos = ref([])
const newTodo = ref({
  description: '',
  course: '',
  dueDate: ''
})
const showOnlyIncomplete = ref(false)
let nextId = 1

const addTodo = () => {
  if (newTodo.value.description.trim() && newTodo.value.course && newTodo.value.dueDate) {
    todos.value.push({
      id: nextId++,
      description: newTodo.value.description.trim(),
      course: newTodo.value.course,
      dueDate: newTodo.value.dueDate,
      completed: false
    })
    
    newTodo.value = {
      description: '',
      course: '',
      dueDate: ''
    }
  }
}

const removeTodo = (id) => {
  const index = todos.value.findIndex(todo => todo.id === id)
  if (index !== -1) {
    todos.value.splice(index, 1)
  }
}

const toggleComplete = (id) => {
  const todo = todos.value.find(todo => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const filteredTodos = computed(() => {
  if (showOnlyIncomplete.value) {
    return todos.value.filter(todo => !todo.completed)
  }
  return todos.value
})

const toggleFilter = () => {
  showOnlyIncomplete.value = !showOnlyIncomplete.value
}
</script>

<template>
  <div class="app-container">
    <div class="hero-section">
      <div class="container">
        <div class="row">
          <div class="col">
            <h1 class="hero-title">📋 My Todo List</h1>
            <p class="hero-subtitle">Stay organized and productive</p>
          </div>
        </div>
      </div>
    </div>

    <div class="container mt-5">
      <div class="row mb-5">
        <div class="col-lg-8 offset-lg-2">
          <div class="add-todo-card">
            <div class="card-header-custom">
              <h5 class="card-title-custom"> + Add New Todo Item</h5>
            </div>
            <div class="card-body-custom">
              <form @submit.prevent="addTodo">
                <div class="form-group-custom">
                  <label for="description" class="form-label-custom">Description</label>
                  <input
                    type="text"
                    class="form-input-custom"
                    id="description"
                    v-model="newTodo.description"
                    placeholder="Enter a short description of your to-do item..."
                    required
                  >
                </div>
                
                <div class="form-group-custom">
                  <label for="course" class="form-label-custom">Course</label>
                  <select
                    class="form-select-custom"
                    id="course"
                    v-model="newTodo.course"
                    required
                  >
                    <option value="">Select a course</option>
                    <option v-for="course in courses" :key="course" :value="course">
                      {{ course }}
                    </option>
                  </select>
                </div>
                
                <div class="form-group-custom">
                  <label for="dueDate" class="form-label-custom">Due Date</label>
                  <input
                    type="date"
                    class="form-input-custom"
                    id="dueDate"
                    v-model="newTodo.dueDate"
                    required
                  >
                </div>
                
                <button type="submit" class="btn-primary-custom">
                  <span>📝</span> Add Todo Item
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>

      <div class="row mb-4" v-if="todos.length > 0">
        <div class="col text-center">
          <button @click="toggleFilter" class="btn-filter-custom">
            <span v-if="showOnlyIncomplete">🔍 Show All Items</span>
            <span v-else>✅ Show Only Incomplete Items</span>
          </button>
        </div>
      </div>

      <div class="row">
        <div class="col-lg-8 offset-lg-2">
          <div v-if="filteredTodos.length === 0 && todos.length === 0" class="empty-state">
            <div class="empty-icon">📝</div>
            <h4 class="empty-title">No todo items yet</h4>
            <p class="empty-subtitle">Add your first todo item using the form above!</p>
          </div>
          
          <div v-else-if="filteredTodos.length === 0 && showOnlyIncomplete" class="empty-state success">
            <div class="empty-icon">🎉</div>
            <h4 class="empty-title">All tasks completed!</h4>
            <p class="empty-subtitle">Great job! You've finished all your todo items.</p>
          </div>
          
          <div v-else>
            <div class="todos-header">
              <h3 class="todos-title">
                {{ showOnlyIncomplete ? '⏳ Incomplete' : '📋 All' }} Todo Items 
                <span class="todo-count">{{ filteredTodos.length }}</span>
              </h3>
            </div>
            
            <div class="todos-list">
              <TodoItem
                v-for="todo in filteredTodos"
                :key="todo.id"
                :todo-item="todo"
                @toggle-complete="toggleComplete"
                @remove-todo="removeTodo"
              />
            </div>
          </div>
        </div>
      </div>

      <div class="stats-section">
        <div class="row">
          <div class="col text-center">
            <div class="stats-container">
              <div class="stat-item">
                <span class="stat-number">{{ todos.length }}</span>
                <span class="stat-label">Total</span>
              </div>
              <div class="stat-divider">•</div>
              <div class="stat-item">
                <span class="stat-number">{{ todos.filter(t => t.completed).length }}</span>
                <span class="stat-label">Completed</span>
              </div>
              <div class="stat-divider">•</div>
              <div class="stat-item">
                <span class="stat-number">{{ todos.filter(t => !t.completed).length }}</span>
                <span class="stat-label">Remaining</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #ff6b6b 0%, #ee5a52 50%, #dc4949 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.hero-section {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 3rem 0;
  margin-bottom: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 700;
  color: white;
  text-align: center;
  margin-bottom: 0.5rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.hero-subtitle {
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.9);
  text-align: center;
  margin: 0;
  font-weight: 300;
}

.add-todo-card {
  background: white;
  border-radius: 20px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.add-todo-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
}

.card-header-custom {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
  padding: 1.5rem 2rem;
  border: none;
}

.card-title-custom {
  color: white;
  font-size: 1.3rem;
  font-weight: 600;
  margin: 0;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
}

.card-body-custom {
  padding: 2rem;
  background: white;
}

.form-group-custom {
  margin-bottom: 1.5rem;
}

.form-label-custom {
  display: block;
  font-weight: 600;
  color: #333;
  margin-bottom: 0.5rem;
  font-size: 1rem;
}

.form-input-custom,
.form-select-custom {
  width: 100%;
  padding: 0.8rem 1rem;
  border: 2px solid #e9ecef;
  border-radius: 12px;
  font-size: 1rem;
  transition: all 0.3s ease;
  background: white;
}

.form-input-custom:focus,
.form-select-custom:focus {
  outline: none;
  border-color: #ff6b6b;
  box-shadow: 0 0 0 3px rgba(255, 107, 107, 0.1);
  transform: translateY(-1px);
}

.btn-primary-custom {
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
  border: none;
  padding: 1rem 2rem;
  border-radius: 12px;
  color: white;
  font-weight: 600;
  font-size: 1.1rem;
  width: 100%;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 107, 107, 0.3);
}

.btn-primary-custom:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 107, 107, 0.4);
}

.btn-primary-custom:active {
  transform: translateY(0);
}

.btn-filter-custom {
  background: white;
  border: 2px solid #ff6b6b;
  padding: 0.8rem 1.5rem;
  border-radius: 25px;
  color: #ff6b6b;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.btn-filter-custom:hover {
  background: #ff6b6b;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 107, 107, 0.3);
}

.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  background: white;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.empty-state.success {
  background: linear-gradient(135deg, #51cf66, #37c940);
  color: white;
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.empty-title {
  font-size: 1.8rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #333;
}

.empty-state.success .empty-title {
  color: white;
}

.empty-subtitle {
  font-size: 1.1rem;
  color: #666;
  margin: 0;
}

.empty-state.success .empty-subtitle {
  color: rgba(255, 255, 255, 0.9);
}

.todos-header {
  margin-bottom: 2rem;
}

.todos-title {
  font-size: 2rem;
  font-weight: 700;
  color: white;
  text-align: center;
  margin-bottom: 1rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.todo-count {
  background: white;
  color: #ff6b6b;
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 600;
  margin-left: 0.5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.todos-list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.stats-section {
  margin-top: 3rem;
  padding: 2rem 0;
}

.stats-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 1.5rem 2rem;
  border-radius: 20px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
}

.stat-number {
  font-size: 2rem;
  font-weight: 700;
  color: white;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
}

.stat-label {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.8);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  font-weight: 500;
}

.stat-divider {
  color: rgba(255, 255, 255, 0.5);
  font-size: 1.5rem;
  font-weight: 300;
}

@media (max-width: 768px) {
  .hero-title {
    font-size: 2.5rem;
  }
  
  .hero-subtitle {
    font-size: 1rem;
  }
  
  .stats-container {
    flex-direction: column;
    gap: 1rem;
  }
  
  .stat-divider {
    display: none;
  }
  
  .card-body-custom {
    padding: 1.5rem;
  }
}
</style>

