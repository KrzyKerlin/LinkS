<template>
    <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center bg-gray-500/75 transition-opacity z-50">
        <div class="absolute inset-0 bg-gradient-to-br from-purple-500 to-blue-500 border-indigo-500 bg-opacity-10" @click="closeModal"></div>
      
        <!-- Modal -->
        <div class="bg-white rounded-lg shadow-xl w-full max-w-md mx-4 p-6 md:p-8 z-10 relative">
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-semibold text-indigo-700">Add New Link</h2>
                <button @click="closeModal" class="text-gray-500 hover:text-gray-700 focus:outline-none">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>
  
            <form @submit.prevent="submitForm" class="space-y-6">
                <div>
                    <label for="title" class="block text-sm font-medium text-indigo-600 mb-2">Title</label>
                    <input id="title" v-model="linkData.title" placeholder="Enter title..." required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                </div>
                <div>
                    <label for="url" class="block text-sm font-medium text-indigo-600 mb-2">URL</label>
                    <input id="url" v-model="linkData.url" placeholder="Enter URL..." required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                </div>  
                <!-- Category options -->
                <div>
                    <label class="block text-sm font-medium text-indigo-600 mb-2">Category</label>
                    <div class="flex space-x-4 justify-center">
                        <!-- WATCH option -->
                        <label class="relative flex-1 cursor-pointer max-w-xs">
                            <input type="radio" name="category" value="WATCH" v-model="linkData.category" class="absolute opacity-0 w-0 h-0" />
                            <div :class="linkData.category === 'WATCH' ? 'bg-gradient-to-br from-purple-500 to-blue-500 border-indigo-500 text-white' : 'bg-white hover:bg-gray-50 border-gray-300 text-gray-700'"
                            class="flex flex-col items-center justify-center py-2 px-2 border-2 rounded-lg transition-all duration-200">
                                <span class="text-xl mb-1">▶</span>
                                <span class="font-medium text-sm">WATCH</span>
                            </div>
                        </label>        
                        <!-- READ option -->
                        <label class="relative flex-1 cursor-pointer max-w-xs">
                            <input type="radio" name="category" value="READ" v-model="linkData.category" class="absolute opacity-0 w-0 h-0" />
                            <div :class="linkData.category === 'READ' ? 'bg-gradient-to-br from-purple-500 to-blue-500 border-indigo-500 text-white' : 'bg-white hover:bg-gray-50 border-gray-300 text-gray-700'"
                            class="flex flex-col items-center justify-center py-2 px-2 border-2 rounded-lg transition-all duration-200">
                                <span class="text-xl mb-1">📄</span>
                                <span class="font-medium text-sm">READ</span>
                            </div>
                        </label>
                    </div>
                </div>
      
                <!-- Submit button -->
                <div class="flex justify-center mt-8">
                    <button type="submit" class="px-8 py-3 bg-indigo-500 text-white rounded-lg hover:bg-gradient-to-br from-purple-500 to-blue-500 transition-colors flex items-center space-x-2 group cursor-pointer">
                        <span class="group-hover:text-white">+</span>
                        <span class="group-hover:text-white">Add Link</span>
                    </button>
                </div>
            </form>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { ref } from 'vue';
  
interface LinkData {
    title: string;
    url: string;
    category: 'WATCH' | 'READ';
}
  
const props = defineProps<{
    isOpen: boolean;
}>();
  
const emit = defineEmits(['close', 'addLink']);
  
const linkData = ref<LinkData>({
    title: '',
    url: '',
    category: 'WATCH'
});
  
const closeModal = () => {
    emit('close');
};
  
const submitForm = () => {
    // Prevent adding empty links
    if (!linkData.value.title.trim() || !linkData.value.url.trim()) return;
  
    // Emit event with link data to parent component
    emit('addLink', { ...linkData.value });
    
    // Reset form
    linkData.value = {
      title: '',
      url: '',
      category: 'WATCH'
    };
    
    // Close modal
    closeModal();
};
</script>