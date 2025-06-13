<template>
  <div class="bg-white p-6 rounded-xl border border-gray-200 shadow-sm">
    <div class="flex items-center">
      <div class="p-3 rounded-lg" :class="iconBgClass">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" :class="iconColor" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" :d="iconPath" />
        </svg>
      </div>
      <div class="ml-4">
        <p class="text-sm font-medium text-gray-500">{{ title }}</p>
        <h3 class="text-xl font-bold text-gray-900">{{ value }}</h3>
        <p :class="`text-sm ${trendingUp ? 'text-green-500' : 'text-red-500'} flex items-center`">
          <svg class="w-3 h-3 mr-1" fill="currentColor" viewBox="0 0 20 20">
            <path v-if="trendingUp" fill-rule="evenodd" d="M12 7a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0V8.414l-4.293 4.293a1 1 0 01-1.414 0L8 10.414l-4.293 4.293a1 1 0 01-1.414-1.414l5-5a1 1 0 011.414 0L11 10.586 14.586 7H12z" clip-rule="evenodd"></path>
            <path v-else fill-rule="evenodd" d="M12 13a1 1 0 110 2H7a1 1 0 01-1-1v-5a1 1 0 112 0v2.586l4.293-4.293a1 1 0 011.414 0L16 9.586l4.293-4.293a1 1 0 111.414 1.414l-5 5a1 1 0 01-1.414 0L13 9.414l-2.293 2.293A1 1 0 0110 12H8a1 1 0 01-1-1v-1H5a1 1 0 110-2h3a1 1 0 011 1v1h.586L12 7.414V5a1 1 0 112 0v2.586l.707-.707a1 1 0 111.414 1.414L13 11.414V13z" clip-rule="evenodd"></path>
          </svg>
          {{ trend }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  title: {
    type: String,
    required: true
  },
  value: {
    type: String,
    required: true
  },
  trend: {
    type: String,
    default: ''
  },
  trendingUp: {
    type: Boolean,
    default: true
  },
  iconPath: {
    type: String,
    required: true
  },
  iconBgClass: {
    type: String,
    default: 'bg-primary-100'
  },
  iconColor: {
    type: String,
    default: 'text-primary-600'
  }
});
</script>