<template>
  <div class="multiselect-container">
    <MultiSelect
      class="multiselect"
      v-model="internalSelectedValues"
      :options="computedOptions"
      optionLabel="label"
      placeholder="Выберите элементы"
      :maxSelectedLabels="maxSelectedLabels"
    />
    <i 
      v-if="internalSelectedValues.length > 0"
      class="pi pi-times clear-icon"
      @click="resetSelection"
    ></i>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, defineEmits, watch, computed } from 'vue';
import MultiSelect from 'primevue/multiselect';

interface Option {
  label: string;
  value: string;
}

const props = defineProps<{
  options?: Option[];
  modelValue?: Option[];
  placeholder?: string;
  maxSelectedLabels?: number;
}>();

const emit = defineEmits<{
  (e: 'update:modelValue', value: Option[]): void;
}>();

const defaultOptions: Option[] = [];
const defaultSelectedValues: Option[] = [];

const internalSelectedValues = ref<Option[]>(props.modelValue ?? defaultSelectedValues);

const computedOptions = computed(() => props.options ?? defaultOptions);

watch(() => props.modelValue, (newValue) => {
  internalSelectedValues.value = newValue ?? defaultSelectedValues;
});

function resetSelection(): void {
  internalSelectedValues.value = [];
  emit('update:modelValue', []);
}
</script>

<style scoped lang="scss">
.multiselect-container {
  position: relative;
  display: flex;
  align-items: center;
}

.clear-icon {
  position: absolute;
  right: 35px;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
}

.multiselect {
  width: 100%;
}
</style>