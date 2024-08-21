<template>
  <div class="link-field">
    <div v-if="!isEditing" class="link-display">
      <template v-if="isLoading">
        <span>Loading...</span>
      </template>
      <template v-else>
        <a :href="absoluteLink" target="_blank" v-text="title || link"></a>
        <Button icon="pi pi-pencil" @click="startEditing" :disabled="isLoading" />
      </template>
    </div>
    <InputText
      v-else
      v-model="inputValue"
      @blur="exitEditingMode"
      @keydown.enter="exitEditingMode"
      placeholder="https://"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed, type Ref } from 'vue';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import axios from 'axios';

interface LinkPreviewResponse {
  title: string;
}

const isEditing: Ref<boolean> = ref(true);
const isLoading: Ref<boolean> = ref(false);
const inputValue: Ref<string> = ref('');
const title: Ref<string | null> = ref(null);
const link: Ref<string> = ref('');
let previousLink: string = '';

const absoluteLink = computed<string>(() => {
  if (!link.value) return '';
  if (link.value.startsWith('http://') || link.value.startsWith('https://')) {
    return link.value;
  }
  return `http://${link.value}`;
});

function startEditing(): void {
  isEditing.value = true;
  inputValue.value = link.value;
}

function exitEditingMode(): void {
  if (inputValue.value === previousLink) {
    isEditing.value = false;
    return;
  }

  link.value = inputValue.value;
  previousLink = link.value;
  isEditing.value = false;
  fetchTitle();
}

async function fetchTitle(): Promise<void> {
  if (!link.value) {
    return;
  }

  isLoading.value = true;
  try {
    const response = await axios.get<LinkPreviewResponse>(
      `https://api.linkpreview.net/?key=d728bbd5039998ddc12044dab64c0f54&q=${link.value}`
    );
    title.value = response.data.title;
  } catch (error) {
    console.error('Failed to fetch title:', error);
    title.value = null;
  } finally {
    isLoading.value = false;
  }
}
</script>

<style scoped lang="scss">
.link-field {
  display: flex;
  align-items: center;
  position: relative;
  
  .link-display {
    display: flex;
    align-items: center;
    
    a {
      text-decoration: none;
      color: #007bff;
    }
    
    .p-button {
      margin-left: 8px;
    }
  }
  
  .p-inputtext {
    flex: 1;
  }
}
</style>
