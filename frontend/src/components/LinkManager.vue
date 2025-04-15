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
                            <div class="flex space-x-8">
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
                    <div class="flex justify-center gap-4 mb-10">
                        <button @click="setFilter('all')" class="relative flex items-center justify-center w-12 h-12 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
                        :class="activeFilter === 'all' ? 'bg-indigo-500 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" aria-label="All links">
                            <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <circle cx="12" cy="12" r="10"></circle>
                                <line x1="2" y1="12" x2="22" y2="12"></line>
                                <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path> 
                            </svg>
                        </button>
                        <button @click="setFilter('favorites')" class="relative flex items-center justify-center w-12 h-12 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
                        :class="activeFilter === 'favorites' ? 'bg-yellow-500 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" aria-label="Favorites links">
                            <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon>
                            </svg>
                        </button>
                        <button @click="setFilter('watch')" class="relative flex items-center justify-center w-12 h-12 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
                        :class="activeFilter === 'watch' ? 'bg-purple-600 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" aria-label="Links to watch">
                            <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <polygon points="23 7 16 12 23 17 23 7"></polygon>
                                <rect x="1" y="5" width="15" height="14" rx="2" ry="2"></rect>
                            </svg>
                        </button>
                        <button @click="setFilter('read')" class="relative flex items-center justify-center w-12 h-12 rounded-full transition-all duration-300 ease-in-out hover:transform hover:-translate-y-1 hover:shadow-md cursor-pointer"
                        :class="activeFilter === 'read' ? 'bg-green-600 text-white shadow-lg' : 'bg-gray-100 hover:bg-gray-200'" aria-label="Links to read">
                            <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <path d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z"></path>
                                <path d="M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z"></path>
                            </svg>
                        </button>   
                    </div>
                </div>

                <!-- Links List-->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mt-4">
                    <div v-for="link in filteredLinks" :key="link.id" 
                    class="bg-white/80 backdrop-blur-lg rounded-xl shadow-md p-4 flex flex-col hover:bg-white/90 transition-all duration-300 border border-gray-100 h-full relative"
                    :class="{'opacity-50': isDragging && draggedItem === link.id}"
                    draggable="true"
                    @dragstart="dragStart($event, link.id)"
                    @dragover="dragOver"
                    @dragenter="dragEnter"
                    @drop="dragDrop($event, link.id)"
                    @dragend="dragEnd">
                        <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" class="absolute top-0 left-2 cursor-move text-indigo-500 mb-2 h-5 w-5">
                            <path d="M5 14H19M5 10H19" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                        </svg>
                        <span :class="{'bg-purple-600': link.category === 'WATCH', 'bg-green-600': link.category === 'READ' }" 
                        class="absolute -top-2 -right-2 text-white px-3 py-1 rounded-md text-xs uppercase font-medium shadow-md transform rotate-2">
                        {{ link.category }}
                        </span>
                        <div class="flex items-start align-center my-2">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-indigo-500 mr-2 mt-1 flex-shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" />
                            </svg>
                            <h3 class="text-lg font-bold text-indigo-500 line-clamp-2 mt-2 pr-2">{{ link.title }}</h3>
                        </div>
                        <a :href="link.url" target="_blank" class="text-md text-indigo-500 hover:font-bold truncate flex items-center justify-center mb-4">
                            <span class="truncate">{{ link.url }}</span>
                        </a>
                        <div class="link-actions mt-auto flex justify-center gap-4">
                            <button @click="toggleFavorite(link.id)" class="p-2 rounded-full bg-gray-100 text-gray-500 hover:text-white transition-colors duration-200 flex items-center justify-center w-8 h-8 cursor-pointer" aria-label="Toggle favorite">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" :class="{'text-yellow-500 fill-current': link.favorite}"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11.049 2.927c.3-.921 1.603-.921 1.902 0l1.519 4.674a1 1 0 00.95.69h4.915c.969 0 1.371 1.24.588 1.81l-3.976 2.888a1 1 0 00-.363 1.118l1.518 4.674c.3.922-.755 1.688-1.538 1.118l-3.976-2.888a1 1 0 00-1.176 0l-3.976 2.888c-.783.57-1.838-.197-1.538-1.118l1.518-4.674a1 1 0 00-.363-1.118l-3.976-2.888c-.784-.57-.38-1.81.588-1.81h4.914a1 1 0 00.951-.69l1.519-4.674z" />
                                </svg>
                            </button>
                            <button @click="shareLink(link)" class="p-2 rounded-full hover:bg-gray-100 transition-colors text-blue-500 cursor-pointer" aria-label="Share link">
                                <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                    <circle cx="18" cy="5" r="3"></circle>
                                    <circle cx="6" cy="12" r="3"></circle>
                                    <circle cx="18" cy="19" r="3"></circle>
                                    <line x1="8.59" y1="13.51" x2="15.42" y2="17.49"></line>
                                    <line x1="15.41" y1="6.51" x2="8.59" y2="10.49"></line>
                                </svg> 
                            </button>
                            <button @click="confirmDelete(link.id)" class="p-2 rounded-full bg-gray-100 hover:bg-red-500 text-gray-500 hover:text-white transition-colors duration-200 flex items-center justify-center w-8 h-8 cursor-pointer" aria-label="Delete link">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                </svg>
                            </button>
                        </div>
                    </div>
                    <!-- URL Message for share link -->
                    <Toast :message="toastMessage" :visible="toastVisible" :is-error="toastIsError" />
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
                        <h3 class="font-bold text-lg text-white">Confirm Delete</h3>
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
    import { ref, computed, onMounted, watch } from 'vue'
    import Toast from './ToastMessageUrl.vue';
  
    interface Link {
        id: number
        title: string
        url: string
        category: 'WATCH' | 'READ'
        order?: number
        favorite?: boolean
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
    
    // Loading links from localStorage when starting the app
    onMounted(() => {
        const savedLinks = localStorage.getItem('myLinks');
        if (savedLinks) {
            links.value = JSON.parse(savedLinks);
        }
        // Remembering filtering in localStorage
        const savedFilter = localStorage.getItem('activeFilter');
        if (savedFilter) {
            activeFilter.value = savedFilter;
        }
    });
    
    // Automatically save links to localStorage 
    watch(links, (newLinks) => {
        localStorage.setItem('myLinks', JSON.stringify(newLinks));
    }, { deep: true });
  
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
            order: links.value.length + 1,
            favorite: false
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
        if (activeFilter.value === 'favorites') {
            return links.value.filter(link => link.favorite === true);
        }
        return links.value.filter(link => 
            link.category === activeFilter.value.toUpperCase()
        )
    })

    const setFilter = (filter) => {
        activeFilter.value = filter
        // Saving active filter to localStorage
        localStorage.setItem('activeFilter', filter);
    }

    // Drag and drop links
    const draggedItem = ref(null);
    const isDragging = ref(false);
    const dragStart = (event, linkId) => {
        draggedItem.value = linkId;
        isDragging.value = true;
        event.dataTransfer.effectAllowed = 'move';
    };
    const dragOver = (event) => {
        event.preventDefault();
    };

    const dragEnter = (event) => {
        event.preventDefault();
    };
    const dragDrop = (event, targetLinkId) => {
        event.preventDefault();
        if (draggedItem.value !== targetLinkId) {
            // Find indexes of links
            const draggedIndex = links.value.findIndex(link => link.id === draggedItem.value);
            const targetIndex = links.value.findIndex(link => link.id === targetLinkId);
        
            if (draggedIndex !== -1 && targetIndex !== -1) {
                // Move link
                const [draggedLink] = links.value.splice(draggedIndex, 1);
                links.value.splice(targetIndex, 0, draggedLink);
                links.value.forEach((link, index) => {
                    link.order = index + 1;
                });
            }
        }
        isDragging.value = false;
    };

    const dragEnd = () => {
        draggedItem.value = null;
        isDragging.value = false;
    };

    // Function to switch favorite link
    const toggleFavorite = (id: number) => {
        const linkIndex = links.value.findIndex(link => link.id === id);
        if (linkIndex !== -1) {
            const updatedLink = { ...links.value[linkIndex] };
            updatedLink.favorite = !updatedLink.favorite;
            links.value[linkIndex] = updatedLink;
        }
    }

    // Checking if the URL is correct
    const isValidURL = (url) => {
        try {
            new URL(url);
            return true;
        } catch (error) {
            return false;
        }
    };

    // Share link with URL validation
    function shareLink(link) {
        if (!isValidURL(link.url)) {
            showToast('Invalid URL');
            return;
        }
        if (navigator.share) {
            navigator.share({
            title: link.title,
            url: link.url
            })
        .then(function() {
            console.log('Link udostępniony!');
        })
        .catch(function(error) {
            console.error('Błąd udostępniania:', error);
        });
        } else {
        // copy to clipboard
        navigator.clipboard.writeText(link.url)
            .then(function() {
                showToast('Link copy to clipboard!');
            })
            .catch(function(err) {
                console.error('Nie można skopiować:', err);
            });
        }
    }

    const toastVisible = ref(false);
    const toastMessage = ref('');
    const toastIsError = ref(false);
    const showToast = (message, error = false) => {
        toastMessage.value = message;
        toastIsError.value = error;
        toastVisible.value = true;
  
        setTimeout(() => {
            toastVisible.value = false;
        }, 3000);
    };
    
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