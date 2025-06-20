<template>
    <div class="min-h-screen bg-gradient-to-br from-sky-400 to-emerald-400 py-10">
        <div class="container mx-auto max-w-4xl px-4">
            <div class="bg-white/80 backdrop-blur-lg rounded-2xl shadow-lg p-6">
                <h1 class="text-5xl font-extrabold text-center mb-8 bg-gradient-to-r from-sky-400 to-emerald-400 bg-clip-text text-transparent leading-tight">
                My <span class="animate-bounce inline-block bg-gradient-to-r from-sky-600 to-emerald-600 bg-clip-text text-transparent">L</span>inkS
                </h1>
                <div class="flex justify-center">
                    <button @click="openAddLinkModal" class="px-4 py-2 bg-gradient-to-r from-sky-600 to-emerald-600 text-white rounded-lg hover:bg-sky-600 transition-colors flex items-center space-x-2 cursor-pointer animate-bounce">
                            <span>+</span> <h2 class="text-xl">Add Link</h2>
                    </button>
                </div>
                <!-- Filter By Category -->
                <CategoryFilter :activeFilter="activeFilter" :totalCount="links.length" :favoriteCount="favoriteCount" :readCount="readCount" :watchCount="watchCount" @setFilter="setFilter" />
                <div class="mt-4 flex justify-center">
                    <Search @search="handleSearch" />
                </div>
            </div>            
        </div>
        <!-- Links List-->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8 mt-8 mx-auto lg:max-w-full px-12 lg:px-20">
            <div v-for="link in filteredLinks" :key="link.id" 
            class="bg-white/80 backdrop-blur-lg rounded-xl shadow-md p-6 flex flex-col hover:bg-white/90 transition-all duration-300 border border-gray-100 h-60 relative"
            :class="{'opacity-50': isDragging && draggedItem === link.id}"
            draggable="true"
            @dragstart="dragStart($event, link.id)"
            @dragover="dragOver"
            @dragenter="dragEnter"
            @drop="dragDrop($event, link.id)"
            @dragend="dragEnd">
                <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" class="absolute top-0 left-2 cursor-move text-sky-600 mb-2 h-5 w-5"> <path d="M5 14H19M5 10H19" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"></path>
                </svg>
                <span :class="{'bg-sky-400': link.category === 'WATCH', 'bg-emerald-400': link.category === 'READ' }" 
                class="absolute -top-2 -right-2 text-white px-3 py-1 rounded-md text-xs uppercase font-medium shadow-md transform rotate-2">
                {{ link.category }}
                </span>
                <div class="flex items-center justify-center h-16 mb-2">
                    <h3 class="text-2xl font-bold text-sky-600 line-clamp-2 overflow-hidden">{{ link.title }}</h3>
                </div>
                <div class="flex items-start h-8">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-sky-600 mr-2 mt-1 flex-shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" />
                    </svg>
                    <a :href="link.url" target="_blank" class="text-md text-center text-sky-600 hover:font-bold truncate block">
                    {{ link.url }}
                    </a>
                </div>
        
                <!-- Tags -->
                <div class="flex-grow overflow-hidden max-h-16">
                    <div v-if="link.tags && link.tags.length > 0" class="flex flex-wrap gap-2 mt-4">
                        <span v-for="(tag, index) in link.tags" :key="index" 
                        class="px-2 py-1 bg-gray-100 text-sky-600 text-xs rounded-md hover:bg-gray-200 transition-colors">
                        {{ tag }}
                        </span>
                    </div>
                </div>
        
                <!-- Actions -->
                <div class="link-actions flex justify-center gap-4 mt-auto pt-2">
                    <button @click="toggleFavorite(link.id)" class="p-2 rounded-full bg-gray-100 text-gray-500 hover:text-white transition-colors duration-200 flex items-center justify-center w-8 h-8 cursor-pointer" aria-label="Toggle favorite">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" :class="{'text-yellow-300 fill-current': link.favorite}"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11.049 2.927c.3-.921 1.603-.921 1.902 0l1.519 4.674a1 1 0 00.95.69h4.915c.969 0 1.371 1.24.588 1.81l-3.976 2.888a1 1 0 00-.363 1.118l1.518 4.674c.3.922-.755 1.688-1.538 1.118l-3.976-2.888a1 1 0 00-1.176 0l-3.976 2.888c-.783.57-1.838-.197-1.538-1.118l1.518-4.674a1 1 0 00-.363-1.118l-3.976-2.888c-.784-.57-.38-1.81.588-1.81h4.914a1 1 0 00.951-.69l1.519-4.674z" />
                        </svg>
                    </button>
                    <button @click="copyLink(link)" class="p-2 rounded-full bg-gray-100 hover:bg-sky-600 text-gray-500 hover:text-white transition-colors duration-200 flex items-center justify-center w-8 h-8 cursor-pointer" aria-label="Copy link to clipboard">
                        <svg v-if="!copiedLink" xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" />
                        </svg>
                        <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                        </svg> 
                    </button>
                    <button @click="confirmDelete(link.id)" class="p-2 rounded-full bg-gray-100 hover:bg-red-500 text-gray-500 hover:text-white transition-colors duration-200 flex items-center justify-center w-8 h-8 cursor-pointer" aria-label="Delete link">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"> <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                        </svg>
                    </button>
                </div>
            </div>
            <!-- URL Message for copy link -->
            <Toast :message="toast.message" :visible="toast.visible" :is-error="toast.isError" />
        </div>
        <!-- Delete Confirmation Modal -->       
        <DeleteLink :isOpen="showConfirmModal" 
        :linkTitle="linkToDelete !== null ? links.find(link => link.id === linkToDelete)?.title : ''"
        @cancel="cancelDelete" @confirm="deleteLink" />
        <!-- Add Link Modal -->
        <AddLinkModal :isOpen="showAddLinkModal" :existingLinks="links" @close="closeAddLinkModal" @addLink="addLink" />
    </div>
</template>
  
<script setup lang="ts">
    import { ref, computed, onMounted, watch, reactive } from 'vue';
    import AddLinkModal from './AddLink.vue';
    import CategoryFilter from './CategoryFilter.vue';
    import Toast from './ToastMessageUrlLink.vue';
    import DeleteLink from './ModalDeleteLink.vue';
    import Search from './Search.vue';
  
    interface Link {
        id: number
        title: string
        url: string
        category: 'WATCH' | 'READ'
        order?: number
        favorite?: boolean
        tags?: string[];
    }

    // Ref variables
    const links = ref<Link[]>([]);
    const showAddLinkModal = ref(false);
    const showConfirmModal = ref(false);
    const activeFilter = ref('all');
    const linkToDelete = ref<number | null>(null);
    const copiedLink = ref(false);
    const draggedItem = ref(null);
    const isDragging = ref(false);
    const searchTerm = ref('');
    // Toast notifications
    const toast = reactive({
        visible: false,
        message: '',
        isError: false
    }); 
    
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

    const handleSearch = (term) => {
        searchTerm.value = term;
    };
    
    // Open and close add link modal
    const openAddLinkModal = () => {
        showAddLinkModal.value = true;
    };
    const closeAddLinkModal = () => {
        showAddLinkModal.value = false;
    };
  
    // Function adding a new link
    const addLink = (newLinkData) => {
        // Add https:// if not present
        if (!newLinkData.url.startsWith('http://') && !newLinkData.url.startsWith('https://')) {
            newLinkData.url = 'https://' + newLinkData.url;
        }
        // Creates a new link object
        const link: Link = {
            ...newLinkData,
            id: Date.now(),
            order: links.value.length + 1,
            favorite: false
        }
        // Adds a new link to the links array
        links.value.push(link);
    };
        
    // Automatically save links to localStorage 
    watch(links, (newLinks) => {
        localStorage.setItem('myLinks', JSON.stringify(newLinks));
    }, { deep: true });

    // Filtered links 
    const filteredLinks = computed(() => {
        let result = links.value;
        // Filter by category
        if (activeFilter.value === 'favorites') {
            result = result.filter(link => link.favorite === true);
        } else if (activeFilter.value !== 'all') {
            result = result.filter(link => 
            link.category === activeFilter.value.toUpperCase()
            );
        }
  
        // Filter by search phrase
        if (searchTerm.value.trim()) {
            const term = searchTerm.value.toLowerCase().trim();
            result = result.filter(link => 
                link.title.toLowerCase().includes(term) || 
                link.url.toLowerCase().includes(term) || 
                (link.tags && Array.isArray(link.tags) && 
                link.tags.some(tag => tag.toLowerCase().includes(term)))
            );
        }
  
        return result;
    });

    const setFilter = (filter) => {
        activeFilter.value = filter
        // Saving active filter to localStorage
        localStorage.setItem('activeFilter', filter);
    }

    // Statistics
    const favoriteCount = computed(() => {
        return links.value.filter(link => link.favorite === true).length;   
    });
    const readCount = computed(() => {
        return links.value.filter(link => link.category === 'READ').length;
    });
    const watchCount = computed(() => {
        return links.value.filter(link => link.category === 'WATCH').length;
    });

    // Drag and drop links
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

    // Copy link to clipboard with URL validation
    function copyLink(link: Link) {
        if (!isValidURL(link.url)) {
            showToast('Invalid URL', true);
            return;
        }
    
        // Try using Clipboard API first (modern browsers)
        if (navigator.clipboard && navigator.clipboard.writeText) {
            navigator.clipboard.writeText(link.url)
                .then(() => showToast('Link copied to clipboard!'))
                .catch(() => fallbackCopyToClipboard(link.url));
            return;
        }
    
        // Fallback for older browsers
        fallbackCopyToClipboard(link.url);
    }

    // Fallback method for copying to clipboard
    function fallbackCopyToClipboard(text: string) {
        const tempInput = document.createElement('input');
        tempInput.value = text;
        tempInput.style.cssText = 'position:fixed;opacity:0;top:0;left:0';
        document.body.appendChild(tempInput);
    
        try {
            tempInput.focus();
            tempInput.select();
            const successful = document.execCommand('copy');       
            if (successful) {
                showToast('Link copied to clipboard!');
                copiedLink.value = true;
                setTimeout(() => { copiedLink.value = false; }, 2000);
            } else {
                // Show manual copy UI if automatic copy fails
                showToast('Could not copy link. Please copy manually.', true);
                tempInput.style.opacity = '1';
                tempInput.style.zIndex = '999999';
                setTimeout(() => {
                    tempInput.style.opacity = '0';
                    document.body.removeChild(tempInput);
                }, 3000);
            }
        } catch (err) {
            console.error('Error copying text:', err);
            showToast('Failed to copy link', true);
        } finally {
            // Ensure cleanup happens even if there's an error
            document.body.removeChild(tempInput);
        }
    }

    const showToast = (message, error = false) => {
        toast.message = message;
        toast.isError = error;
        toast.visible = true;
  
        setTimeout(() => {
            toast.visible = false;
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