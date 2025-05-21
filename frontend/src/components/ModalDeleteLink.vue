<!-- ConfirmDeleteModal.vue -->
<template>
    <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center bg-gray-500/75 transition-opacity z-50" 
      aria-hidden="true" @click="cancelDelete">
      <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:w-full sm:max-w-lg" @click.stop>

        <div class="modal-header bg-sky-600 px-6 py-4 flex items-center justify-center">
          <div class="flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            <h3 class="font-bold text-lg text-white">Confirm Delete</h3>
          </div>
        </div>
      
        <div class="modal-content px-6 py-8 text-center">
          <p class="text-gray-700 mb-4">Are you sure you want to delete this link?</p>
          <p v-if="linkTitle" class="text-sky-600 font-medium text-lg mt-2">
            "{{ linkTitle }}"
          </p>
        </div>

        <div class="bg-gray-50 px-6 py-4 flex justify-center gap-4">
          <button type="button" @click="cancelDelete" 
          class="rounded-md border border-gray-300 bg-white px-5 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-100 cursor-pointer transition-transform duration-300 ease-out transform hover:scale-105 active:scale-95">
          Cancel
          </button>
          <button type="button" @click="confirmDelete" 
          class="rounded-md bg-gray-200 px-5 py-2 text-sm font-medium text-sky-600 shadow-sm hover:bg-red-600 hover:text-white transition-transform duration-300 ease-in-out flex items-center cursor-pointer transform hover:scale-105 active:scale-95">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
            </svg>
            Delete
          </button>
        </div>
      
      </div>
    </div>
</template>
  
<script setup lang="ts">
  defineProps({
    isOpen: {
      type: Boolean,
      required: true
    },
    linkTitle: {
      type: String,
      default: ''
    }
  });
  
  const emit = defineEmits(['cancel', 'confirm'])
  const cancelDelete = () => {
    emit('cancel')
  }
  const confirmDelete = () => {
    emit('confirm') 
  }
</script>