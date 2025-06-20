<template>
    <div class="mt-8">   
      <div class="flex justify-center gap-4">
        <!-- All filter -->
        <button @click="setFilter('all')" 
          class="relative flex items-center justify-center w-12 h-12 p-2 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
          :class="activeFilter === 'all' ? 'bg-sky-700 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" 
          aria-label="All links">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="12" cy="12" r="10"></circle>
            <line x1="2" y1="12" x2="22" y2="12"></line>
            <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path> 
          </svg>
          {{ totalCount }}
        </button>
        
        <!-- Favorites filter -->
        <button @click="setFilter('favorites')"
          class="relative flex items-center justify-center w-12 h-12 p-2 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
          :class="activeFilter === 'favorites' ? 'bg-yellow-300 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" 
          aria-label="Favorites links">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon>
          </svg>
          {{ favoriteCount }}
        </button>
        
        <!-- Watch filter -->
        <button @click="setFilter('watch')" 
          class="relative flex items-center justify-center w-12 h-12 p-2 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
          :class="activeFilter === 'watch' ? 'bg-sky-400 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" 
          aria-label="Links to watch">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polygon points="23 7 16 12 23 17 23 7"></polygon>
            <rect x="1" y="5" width="15" height="14" rx="2" ry="2"></rect>
          </svg>
          {{ watchCount }}
        </button>
        
        <!-- Read filter -->
        <button @click="setFilter('read')" 
          class="relative flex items-center justify-center w-12 h-12 p-2 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
          :class="activeFilter === 'read' ? 'bg-emerald-400 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" 
          aria-label="Links to read">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path>
            <path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path>
          </svg>
          {{ readCount }}
        </button>   
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  defineProps({
    activeFilter: {
      type: String,
      required: true
    },
    totalCount: {
      type: Number,
      default: 0
    },
    favoriteCount: {
      type: Number,
      default: 0
    },
    readCount: {
      type: Number,
      default: 0
    },
    watchCount: {
      type: Number,
      default: 0
    }
  });
  
  const emit = defineEmits(['setFilter']);
  const setFilter = (filter) => {
    emit('setFilter', filter)
  }
</script>