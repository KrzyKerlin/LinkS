<template>
    <transition name="toast-fade">
      <div 
        v-if="visible" 
        class="toast-message"
        :class="{ 'toast-error': isError }">
        {{ message }}
      </div>
    </transition>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  const props = defineProps({
    message: {
      type: String,
      required: true
    },
    visible: {
      type: Boolean,
      default: false
    },
    isError: {
      type: Boolean,
      default: false
    }
  });
  </script>
  
  <style scoped>
  .toast-message {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background-color: #4CAF50;
    color: white;
    padding: 8px 16px;
    border-radius: 4px;
    font-size: 14px;
    z-index: 999;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  }
  
  .toast-error {
    background-color: #F44336;
  }
  
  .toast-fade-enter-active,
  .toast-fade-leave-active {
    transition: opacity 0.3s, transform 0.3s;
  }
  
  .toast-fade-enter-from,
  .toast-fade-leave-to {
    opacity: 0;
    transform: translate(-50%, 10px);
  }
  </style>