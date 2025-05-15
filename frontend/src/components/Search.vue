<template>
  <div class="search-container relative z-10">
    <button 
      v-if="!isSearchOpen" 
      @click="toggleSearch" 
      class="search-icon-btn p-2 rounded-full bg-white/80 shadow-md hover:bg-white hover:shadow-lg transition-all duration-300 focus:outline-none"
      aria-label="Otwórz wyszukiwarkę"
    >
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-sky-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
        <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
      </svg>
    </button>
    <!-- Animation -->
    <transition 
      enter-active-class="transition-all duration-300 ease-out" 
      enter-from-class="transform scale-x-0 opacity-0"
      enter-to-class="transform scale-x-100 opacity-100"
      leave-active-class="transition-all duration-200 ease-in" 
      leave-from-class="transform scale-x-100 opacity-100"
      leave-to-class="transform scale-x-0 opacity-0"
    >
      <div 
        v-show="isSearchOpen" 
        class="search-input-container absolute top-0 right-0 flex items-center overflow-hidden bg-white rounded-full border-2 border-sky-400 shadow-md origin-right w-56"
      >
        <input 
          id="search-input"
          type="text" 
          v-model="searchTerm" 
          placeholder="Search..." 
          class="w-full py-2 px-4 outline-none bg-transparent text-gray-700" 
          @keydown="handleKeydown"
        />
        <button 
          @click="closeSearch" 
          class="p-2 text-gray-400 hover:text-gray-600 focus:outline-none transition-colors"
          aria-label="Close"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, watch, onBeforeUnmount, nextTick } from 'vue';

const isSearchOpen = ref(false);
const searchTerm = ref('');
const emit = defineEmits(['search']);

const toggleSearch = () => {
  isSearchOpen.value = !isSearchOpen.value;
  if (isSearchOpen.value) {
    nextTick(() => {
      document.getElementById('search-input')?.focus();
    });
  }
};

const clearSearch = () => {
  searchTerm.value = '';
  emit('search', '');
};

const closeSearch = () => {
  isSearchOpen.value = false;
  clearSearch(); 
};

const handleKeydown = (event) => {
  if (event.key === 'Escape') {
    closeSearch();
  }
};

const handleDocumentKeydown = (event) => {
  if (event.key === 'Escape' && isSearchOpen.value) {
    closeSearch();
  }
};

watch(searchTerm, (newValue) => {
  emit('search', newValue);
}, { immediate: true });

onBeforeUnmount(() => {
  document.removeEventListener('keydown', handleDocumentKeydown);
});
</script>