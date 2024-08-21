<template>
  <div v-if="formattedTime">Значение переменной директивы v-model: {{ formattedTime }}</div>
  <Calendar 
    class="time-picker"
    v-model="time"
    hourFormat="24"
    placeholder="00:00"  
    timeOnly 
    showTime   
    showIcon
    showButtonBar
  />
</template>

<script setup lang="ts">
import { ref, computed, type Ref } from 'vue';
import Calendar from 'primevue/calendar';

const time: Ref<Date | null> = ref(null);

const formattedTime = computed(() => {
  if (!time.value) return '';
  const hours = time.value.getHours().toString().padStart(2, '0');
  const minutes = time.value.getMinutes().toString().padStart(2, '0');
  return `${hours}:${minutes}`;
});
</script>

<style scoped lang="scss">
.time-picker {
  width: 100%;
}
</style>

