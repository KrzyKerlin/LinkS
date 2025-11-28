<template>
  <div class="mt-8">         
    <!-- Category Filters -->
    <div class="flex items-center justify-center gap-8">
      <!-- All filter -->
      <button @click="setFilter('all')" class="group relative flex flex-col items-center gap-1 px-4 py-3    rounded-xl transition-all duration-300 cursor-pointer"
      :class="activeFilter === 'all' 
      ? 'bg-gradient-to-br from-sky-800 to-sky-900 text-white shadow-lg shadow-sky-500/30' : 'bg-gray-100 text-gray-600 hover:bg-sky-800 hover:text-white'" aria-label="All links">
        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><line x1="2" y1="12" x2="22" y2="12"></line><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path>
        </svg>
          <span class="text-xs font-bold">{{ totalCount }}</span>
      </button>

      <!-- Favorites filter -->
      <button @click="setFilter('favorites')" class="group relative flex flex-col items-center gap-1 px-4 py-3 rounded-xl transition-all duration-300 cursor-pointer"
      :class="activeFilter === 'favorites' 
      ? 'bg-gradient-to-br from-yellow-400 to-amber-500 text-white shadow-lg shadow-amber-500/30' : 'bg-gray-100 text-gray-600 hover:bg-yellow-400 hover:text-white'" aria-label="Favorite links">
        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" viewBox="0 0 24 24" :fill="activeFilter === 'favorites' ? 'currentColor' : 'none'" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon>
        </svg>
        <span class="text-xs font-bold">{{ favoriteCount }}</span>
      </button>

      <!-- Watch filter -->
      <button @click="setFilter('watch')" class="group relative flex flex-col items-center gap-1 px-4 py-3 rounded-xl transition-all duration-300 cursor-pointer"
      :class="activeFilter === 'watch' 
      ? 'bg-gradient-to-br from-sky-400 to-sky-500 text-white shadow-lg shadow-sky-400/30'  : 'bg-gray-100 text-gray-600 hover:bg-sky-400 hover:text-white'" aria-label="Links to watch">
        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="23 7 16 12 23 17 23 7"></polygon><rect x="1" y="5" width="15" height="14" rx="2" ry="2"></rect>
        </svg>
        <span class="text-xs font-bold">{{ watchCount }}</span>
      </button>

      <!-- Read filter -->
      <button @click="setFilter('read')" class="group relative flex flex-col items-center gap-1 px-4 py-3 rounded-xl transition-all duration-300 cursor-pointer"
      :class="activeFilter === 'read' 
      ? 'bg-gradient-to-br from-emerald-400 to-emerald-500 text-white shadow-lg shadow-emerald-400/30'   : 'bg-gray-100 text-gray-600 hover:bg-emerald-400 hover:text-white'" aria-label="Links to read">
        <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path><path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path>
        </svg>
        <span class="text-xs font-bold">{{ readCount }}</span>
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