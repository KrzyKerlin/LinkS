<template>
    <Transition 
      name="modal-animation"
      enter-active-class="duration-300 ease-out"
      enter-from-class="opacity-0 scale-95"
      enter-to-class="opacity-100 scale-100"
      leave-active-class="duration-200 ease-in"
      leave-from-class="opacity-100 scale-100"
      leave-to-class="opacity-0 scale-95"
    >
      <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center z-50">
        <!-- Modal backdrop with fade animation -->
        <div class="absolute inset-0 bg-indigo-500/75 backdrop-blur-sm transition-opacity"></div>
      
        <!-- Modal -->
        <div class="bg-white rounded-xl shadow-md w-full max-w-md mx-4 p-6 md:p-8 z-10 relative"
          @click.stop
        >
            <div class="flex justify-between items-center mb-6">
                <h2 class="text-xl font-semibold text-indigo-700 animate-bounce">Add New Link</h2>
                <button @click="closeModal" class="text-gray-500 hover:text-indigo-700 focus:outline-none cursor-pointer">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>
  
            <form @submit.prevent="submitForm" class="space-y-6">
                <div class="relative">
                    <input type="text" id="title" v-model="linkData.title" 
                    class="block px-2.5 pb-1.5 pt-3 w-full text-sm text-gray-900 bg-transparent rounded-lg border border-gray-300 appearance-none focus:outline-none focus:ring-0 focus:border-indigo-600 peer" 
                    placeholder=" " required />
                    <label for="title" class="absolute text-sm text-indigo-600 duration-300 transform -translate-y-3 scale-75 top-1 z-10 origin-[0] bg-white px-2 peer-focus:px-2 peer-focus:text-indigo-600 peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-1 peer-focus:scale-75 peer-focus:-translate-y-3 start-1">
                    Title
                    </label>
                </div>
                <div class="relative">
                    <input type="text" id="url" v-model="linkData.url" 
                    class="block px-2.5 pb-1.5 pt-3 w-full text-sm text-gray-900 bg-transparent rounded-lg border border-gray-300 appearance-none focus:outline-none focus:ring-0 focus:border-indigo-600 peer" 
                    placeholder=" " required @blur="checkDuplicate" />
                    <label for="url" class="absolute text-sm text-indigo-600 duration-300 transform -translate-y-3 scale-75 top-1 z-10 origin-[0] bg-white px-2 peer-focus:px-2 peer-focus:text-indigo-600 peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-1 peer-focus:scale-75 peer-focus:-translate-y-3 start-1">
                    URL Link
                    </label>
                </div>
                <!-- Tags -->
                <div class="relative">
                    <input type="text" id="tags" v-model="tagsInput" 
                    class="block px-2.5 pb-1.5 pt-3 w-full text-sm text-gray-900 bg-transparent rounded-lg border border-gray-300 appearance-none focus:outline-none focus:ring-0 focus:border-indigo-600 peer" 
                    placeholder=" " />
                    <label for="tags" class="absolute text-sm text-indigo-600 duration-300 transform -translate-y-3 scale-75 top-1 z-10 origin-[0] bg-white px-2 peer-focus:px-2 peer-focus:text-indigo-600 peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-1 peer-focus:scale-75 peer-focus:-translate-y-3 start-1">
                    Tags (space separated with #)
                    </label>
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
    </Transition>
  
    <!-- Toast component -->
    <Toast :message="toastMessage" :visible="showToast" :is-error="isToastError" />
</template>
  
<script setup lang="ts">
import { ref } from 'vue';
import Toast from './ToastMessageUrlLink.vue';
  
interface LinkData {
    title: string;
    url: string;
    category: 'WATCH' | 'READ';
    tags?: string[];
}
     
interface Link extends LinkData {
    id?: string | number;
    favorite?: boolean;
    order?: number;
}
  
const props = defineProps<{
    isOpen: boolean;
    existingLinks: Link[];
}>();
  
const emit = defineEmits(['close', 'addLink']);
  
const linkData = ref<LinkData>({
    title: '',
    url: '',
    category: 'WATCH',
    tags: []
  });
  
// Tags input state
const tagsInput = ref('');
  
// Computed property to display tags as they're typed

  
// Toast state
const showToast = ref(false);
const toastMessage = ref('');
const isToastError = ref(false);
  
const displayToast = (message: string, isError: boolean = false) => {
    toastMessage.value = message;
    isToastError.value = isError;
    showToast.value = true;
    setTimeout(() => {
      showToast.value = false;
    }, 3000);
};

const closeModal = () => {
    emit('close');
};
  
// Check for duplicate URLs
const checkDuplicate = () => {
  if (!linkData.value.url || linkData.value.url.trim().length < 3) return false;
  
  // URL normalization in one step
  let normalizedNewUrl = linkData.value.url.trim().toLowerCase();
  if (!/^https?:\/\//i.test(normalizedNewUrl)) {
    normalizedNewUrl = 'https://' + normalizedNewUrl;
  }
  normalizedNewUrl = normalizedNewUrl.replace(/\/+$/, '');
  
  // Check for duplicate
  const existingLink = props.existingLinks.find(link => {
    let normalizedExisting = link.url.trim().toLowerCase();
    if (!/^https?:\/\//i.test(normalizedExisting)) {
      normalizedExisting = 'https://' + normalizedExisting;
    }
    normalizedExisting = normalizedExisting.replace(/\/+$/, '');
    
    return normalizedExisting === normalizedNewUrl;
});
  
  if (existingLink) {
    displayToast(`This link already exists in "${existingLink.category}"!`, true);
    return true;
  }
  
  return false;
};
  
const submitForm = () => {
    // Prevent adding empty links
    if (!linkData.value.title.trim() || !linkData.value.url.trim()) return;
  
    // Check for duplicates
    if (checkDuplicate()) return;
  
   // Process tags before submitting
    const processedTags = tagsInput.value
      .split(' ')
      .filter(tag => tag.trim() !== '')
      .map(tag => tag.startsWith('#') ? tag : `#${tag}`);
  
    // Emit event with link data to parent component
    emit('addLink', { 
      ...linkData.value,
      tags: processedTags.length > 0 ? processedTags : undefined
    });
    
    // Reset form
    linkData.value = {
      title: '',
      url: '',
      category: 'WATCH',
      tags: []
    };
    tagsInput.value = '';
    
    // Close modal
    closeModal();
};
</script>