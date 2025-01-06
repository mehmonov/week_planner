<template>
    <div class="min-h-screen bg-gradient-to-br from-teal-50 to-blue-50 dark:from-gray-900 dark:to-gray-800">
      <div class="container mx-auto px-4 py-8">
        <!-- Stats Section -->
        <div class="mb-8">
          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm">
              <h3 class="text-gray-500 dark:text-gray-400 text-sm font-medium">Tasks Completed</h3>
              <div class="mt-2 flex items-baseline">
                <span class="text-3xl font-bold text-gray-900 dark:text-white">
                  {{ totalCompletedTasks }}
                </span>
                <span class="ml-2 text-gray-600 dark:text-gray-300">/ {{ totalTasks }}</span>
              </div>
              <div class="mt-4 w-full bg-gray-100 dark:bg-gray-700 rounded-full h-2">
                <div
                  class="bg-teal-500 h-2 rounded-full transition-all duration-500"
                  :style="{ width: `${completionRate}%` }"
                ></div>
              </div>
            </div>
  
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm">
              <h3 class="text-gray-500 dark:text-gray-400 text-sm font-medium">Weekly Progress</h3>
              <div class="mt-2">
                <span class="text-3xl font-bold text-gray-900 dark:text-white">{{ completionRate }}%</span>
              </div>
              <p class="mt-2 text-gray-600 dark:text-gray-300">
                {{ daysLeft }} days remaining
              </p>
            </div>
  
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm">
              <h3 class="text-gray-500 dark:text-gray-400 text-sm font-medium">Current Week</h3>
              <div class="mt-2 text-3xl font-bold text-gray-900 dark:text-white">
                {{ currentWeek }}
              </div>
              <p class="mt-2 text-gray-600 dark:text-gray-300">
                {{ currentDate }}
              </p>
            </div>
          </div>
        </div>
  
        <!-- Add Task Form -->
        <div class="mb-8">
          <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm">
            <h2 class="text-xl font-semibold text-gray-900 dark:text-white mb-4">Add Weekly Task</h2>
            <form @submit.prevent="addTask" class="flex flex-col md:flex-row gap-4">
              <select
                v-model="selectedDay"
                class="px-4 py-2 rounded-lg border border-gray-200 dark:border-gray-700 
                       bg-gray-50 dark:bg-gray-900 text-gray-900 dark:text-white
                       focus:ring-2 focus:ring-teal-500 focus:border-transparent"
              >
                <option v-for="day in weekDays" :key="day" :value="day">{{ day }}</option>
              </select>
              <input
                v-model="newTask"
                type="text"
                placeholder="Enter a new task"
                class="flex-1 px-4 py-2 rounded-lg border border-gray-200 dark:border-gray-700 
                       bg-gray-50 dark:bg-gray-900 text-gray-900 dark:text-white
                       focus:ring-2 focus:ring-teal-500 focus:border-transparent"
              />
              <button
                type="submit"
                class="px-6 py-2 bg-teal-500 hover:bg-teal-600 text-white rounded-lg
                       transition duration-200 transform hover:scale-105"
              >
                Add Task
              </button>
            </form>
          </div>
        </div>
  
        <!-- Weekly Tasks -->
        <div class="grid grid-cols-1 md:grid-cols-7 gap-4">
          <div
            v-for="day in weekDays"
            :key="day"
            class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm"
          >
            <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">{{ day }}</h3>
            <transition-group name="task-list" tag="ul" class="space-y-3">
              <li
                v-for="task in getTasksByDay(day)"
                :key="task.id"
                class="flex items-center space-x-3 group"
              >
                <input
                  type="checkbox"
                  :id="'task-' + task.id"
                  v-model="task.completed"
                  class="w-5 h-5 text-teal-500 rounded border-gray-300 
                         focus:ring-teal-500 dark:border-gray-600"
                  @change="updateTask(task)"
                />
                <label
                  :for="'task-' + task.id"
                  class="flex-1 text-gray-700 dark:text-gray-300"
                  :class="{ 'line-through text-gray-400': task.completed }"
                >
                  {{ task.text }}
                </label>
                <button
                  @click="deleteTask(task.id)"
                  class="opacity-0 group-hover:opacity-100 text-gray-400 hover:text-red-500
                         transition-opacity duration-200"
                >
                  <TrashIcon class="h-5 w-5" />
                </button>
              </li>
            </transition-group>
            
            <div
              v-if="getTasksByDay(day).length === 0"
              class="text-center py-4 text-gray-400 dark:text-gray-500"
            >
              No tasks for this day
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  import { TrashIcon } from 'lucide-vue-next'
  
  // State
  const tasks = ref([])
  const newTask = ref('')
  const selectedDay = ref('Monday')
  const weekDays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
  
  // Load tasks from localStorage(temporarly, in future it will be save backend)
  const savedTasks = localStorage.getItem('weeklyTasks')
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks)
  }
  
  // Computed properties
  const currentDate = computed(() => {
    return new Date().toLocaleDateString('en-US', {
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    })
  })
  
  const currentWeek = computed(() => {
    const now = new Date()
    const start = new Date(now.getFullYear(), 0, 1)
    const week = Math.ceil((((now - start) / 86400000) + start.getDay() + 1) / 7)
    return `Week ${week}`
  })
  
  const totalCompletedTasks = computed(() => {
    return tasks.value.filter(task => task.completed).length
  })
  
  const totalTasks = computed(() => tasks.value.length)
  
  const completionRate = computed(() => {
    if (totalTasks.value === 0) return 0
    return Math.round((totalCompletedTasks.value / totalTasks.value) * 100)
  })
  
  const daysLeft = computed(() => {
    const today = new Date().getDay()
    return 7 - (today === 0 ? 7 : today)
  })
  
  // Methods
  const getTasksByDay = (day) => {
    return tasks.value.filter(task => task.day === day)
  }
  
  const addTask = () => {
    if (newTask.value.trim()) {
      const task = {
        id: Date.now(),
        text: newTask.value.trim(),
        completed: false,
        day: selectedDay.value,
        createdAt: new Date()
      }
      tasks.value.push(task)
      newTask.value = ''
      saveTasks()
    }
  }
  
  const updateTask = (task) => {
    const index = tasks.value.findIndex(t => t.id === task.id)
    if (index !== -1) {
      tasks.value[index] = { ...task }
      saveTasks()
    }
  }
  
  const deleteTask = (taskId) => {
    tasks.value = tasks.value.filter(task => task.id !== taskId)
    saveTasks()
  }
  
  const saveTasks = () => {
    localStorage.setItem('weeklyTasks', JSON.stringify(tasks.value))
  }
  </script>
  
  <style scoped>
  .task-list-enter-active,
  .task-list-leave-active {
    transition: all 0.5s ease;
  }
  
  .task-list-enter-from,
  .task-list-leave-to {
    opacity: 0;
    transform: translateX(30px);
  }
  
  .task-list-move {
    transition: transform 0.5s ease;
  }
  
  @media (max-width: 768px) {
    .grid-cols-7 {
      grid-template-columns: repeat(1, 1fr);
    }
  }
  </style>