<template>
    <div class="min-h-screen bg-gradient-to-br from-purple-500 to-blue-500 py-10">
        <div class="container mx-auto max-w-4xl px-4">
            <div class="bg-white/80 backdrop-blur-lg rounded-2xl shadow-lg p-6">
                <h1 class="text-5xl font-extrabold text-center mb-8 bg-gradient-to-r from-indigo-900 to-purple-400 bg-clip-text text-transparent leading-tight">
                My LinkS
                </h1>
          
                <form @submit.prevent="addLink" class="space-y-4">
                    <div class="grid grid-cols-1 md:grid-cols-12 gap-4">
                        <div class="md:col-span-4">
                            <label for="title" class="text-sm font-medium text-indigo-600 mb-2">Title</label>
                            <input id="title" v-model="newLink.title" placeholder="Enter title..." required 
                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                        </div>
              
                        <div class="md:col-span-5">
                            <label for="url" class="text-sm font-medium text-indigo-600 mb-2">URL</label>
                            <input id="url" v-model="newLink.url" placeholder="Enter URL..." required 
                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                        </div>

                        <div class="md:col-span-3">
                            <div class="flex space-x-2">
                                <!-- WATCH option -->
                                <label class="relative flex-1 cursor-pointer">
                                    <input type="radio" name="category" value="WATCH" v-model="newLink.category" class="absolute opacity-0 w-0 h-0" />
                                    <div :class="newLink.category === 'WATCH' ? 
                                    'bg-gradient-to-br from-purple-500 to-blue-500 border-indigo-500 text-white' : 
                                    'bg-white hover:bg-gray-50 border-gray-300 text-gray-700'"
                                    class="flex flex-col items-center justify-center py-3 px-2 border-2 rounded-lg transition-all duration-200">
                                        <span class="text-xl mb-1">▶</span>
                                        <span class="font-medium text-sm">WATCH</span>
                                    </div>
                                </label>        
                                <!-- READ option -->
                                <label class="relative flex-1 cursor-pointer">
                                    <input type="radio" name="category" value="READ" v-model="newLink.category" class="absolute opacity-0 w-0 h-0" />
                                    <div :class="newLink.category === 'READ' ? 
                                    'bg-gradient-to-br from-purple-500 to-blue-500 border-indigo-500 text-white' : 
                                    'bg-white hover:bg-gray-50 border-gray-300 text-gray-700'"
                                    class="flex flex-col items-center justify-center py-3 px-2 border-2 rounded-lg transition-all duration-200">
                                        <span class="text-xl mb-1">📄</span>
                                        <span class="font-medium text-sm">READ</span>
                                    </div>
                                </label>
                            </div>
                        </div>
                    </div>
            
                    <div class="flex justify-center">
                        <button type="submit" class="px-6 py-2 bg-indigo-500 text-gray-100 rounded-lg hover:bg-gradient-to-br from-purple-500 to-blue-500 cursor-pointer transition-colors flex items-center space-x-2 group" >
                            <span class="group-hover:text-white">+</span>
                            <span class="group-hover:text-white">Add Link</span>
                        </button>
                    </div>
                </form>

                <!-- Filter By Category -->
                <div class="mt-8">
                    <h2 class="text-xl font-bold text-indigo-800 mb-4">Filter by Category</h2>
                    <div class="grid grid-cols-3 gap-4 mb-6">
                        <button @click="setFilter('all')" 
                        :class="{'bg-indigo-500 text-white': activeFilter === 'all', 'bg-white text-indigo-500': activeFilter !== 'all'}"
                        class="py-2 px-4 rounded-lg flex items-center justify-center transition-colors cursor-pointer">
                            <span class="mr-2">☰</span> All
                        </button>
                        <button @click="setFilter('watch')" 
                        :class="{'bg-indigo-500 text-white': activeFilter === 'watch', 'bg-white text-indigo-500': activeFilter !== 'watch'}"
                        class="py-2 px-4 rounded-lg flex items-center justify-center transition-colors cursor-pointer">
                            <span class="mr-2">▶</span> Watch
                        </button>
                        <button @click="setFilter('read')" 
                        :class="{'bg-indigo-500 text-white': activeFilter === 'read', 'bg-white text-indigo-500': activeFilter !== 'read'}"
                        class="py-2 px-4 rounded-lg flex items-center justify-center transition-colors cursor-pointer">
                            <span class="mr-2">📄</span> Read
                        </button>
                    </div>
                </div>

                <!-- Links List-->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mt-4">
                    <div v-for="link in filteredLinks" :key="link.id" 
                    class="bg-white/80 backdrop-blur-lg rounded-xl shadow-md p-4 flex flex-col hover:bg-white/90 transition-all duration-300 border border-gray-100 h-full relative">
                        <span :class="{'bg-purple-600': link.category === 'WATCH', 'bg-green-600': link.category === 'READ' }" 
                        class="absolute -top-2 -right-2 text-white px-3 py-1 rounded-md text-xs uppercase font-medium shadow-md transform rotate-2">
                        {{ link.category }}
                        </span>
                        <div class="flex items-start mt-2 mb-3">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500 mr-2 mt-1 flex-shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" />
                            </svg>
                            <h3 class="text-lg font-bold text-indigo-800 line-clamp-2 pr-2">{{ link.title }}</h3>
                        </div>
                        <a :href="link.url" target="_blank" class="text-blue-600 hover:text-blue-800 hover:underline truncate flex items-center mb-4 text-sm">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1 flex-shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                            </svg>
                            <span class="truncate">{{ link.url }}</span>
                        </a>
                        <div class="link-actions mt-auto flex justify-center">
                            <button @click="confirmDelete(link.id)" class="p-2 rounded-full bg-gray-100 hover:bg-red-500 text-gray-500 hover:text-white transition-colors duration-200 flex items-center justify-center w-8 h-8 cursor-pointer" aria-label="Delete link">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Delete Confirmation Modal -->       
        <div v-if="showConfirmModal" class="fixed inset-0 flex items-center justify-center bg-gray-500/75 transition-opacity z-50" aria-hidden="true" @click="cancelDelete">
            <div class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:w-full sm:max-w-lg" @click.stop>
                <div class="modal-header bg-indigo-500 px-6 py-4 flex items-center justify-center">
                    <div class="flex items-center">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                        <h3 class="font-bold text-lg text-white">Confirm Deletion</h3>
                    </div>
                </div>
                <div class="modal-content px-6 py-8 text-center">
                    <p class="text-gray-700 mb-4">Are you sure you want to delete this link?</p>
                    <p v-if="linkToDelete !== null" class="text-indigo-500 font-medium text-lg mt-2">
                    "{{ links.find(link => link.id === linkToDelete)?.title }}"
                    </p>
                </div>
                <div class="bg-gray-50 px-6 py-4 flex justify-center gap-4">
                    <button type="button" @click="cancelDelete" class="rounded-md border border-gray-300 bg-white px-5 py-2 text-sm font-medium text-gray-700 shadow-sm hover:bg-gray-50 cursor-pointer">
                    Cancel
                    </button>
                    <button type="button" @click="deleteLink" class="rounded-md bg-gray-200 px-5 py-2 text-sm font-medium text-indigo-500 shadow-sm hover:bg-red-600 hover:text-white transition-colors duration-200 flex items-center cursor-pointer">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                        </svg>
                        Delete
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">
    import { ref, computed  } from 'vue'
  
    interface Link {
        id: number
        title: string
        url: string
        category: 'WATCH' | 'READ'
        order?: number
    }
  
    const newLink = ref({
        title: '',
        url: '',
        category: 'WATCH' as 'WATCH' | 'READ'
    })

    // Link array - will store all added links
    const links = ref<Link[]>([]);
    const showConfirmModal = ref(false);
    const linkToDelete = ref<number | null>(null);
  
    // Function adding a new link
    const addLink = () => {
        // Prevent adding empty links
        if (!newLink.value.title.trim() || !newLink.value.url.trim()) return

        // Add https:// if not present
        if (!newLink.value.url.startsWith('http://') && !newLink.value.url.startsWith('https://')) {
            newLink.value.url = 'https://' + newLink.value.url
        }
    
        // Creates a new link object
        const link: Link = {
            ...newLink.value,
            id: Date.now(),
            order: links.value.length + 1
        }
       
        // Adds a new link to the links array
        links.value.push(link)   
    
        // Resets the form to empty values
        newLink.value = {
            title: '',
            url: '',
            category: 'WATCH'
        }
    }

    // Filtered links by category 
    const activeFilter = ref('all')
    const filteredLinks = computed(() => {
        if (activeFilter.value === 'all') {
            return links.value
        }
        return links.value.filter(link => 
            link.category === activeFilter.value.toUpperCase()
        )
    })

    const setFilter = (filter) => {
        activeFilter.value = filter
    }

    // Delete link and show modal
    const confirmDelete = (id: number) => {
      linkToDelete.value = id;
      showConfirmModal.value = true;
    };
    
    const cancelDelete = () => {
      showConfirmModal.value = false;
      linkToDelete.value = null;
    };

    const deleteLink = () => {
      if (linkToDelete.value === null) return;
      
      links.value = links.value.filter(link => link.id !== linkToDelete.value);
      showConfirmModal.value = false;
      linkToDelete.value = null;
    };
</script>