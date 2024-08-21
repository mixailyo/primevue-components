<script setup lang="ts">
import LinkField from './components/LinkField.vue'
import TimePicker from './components/TimePicker.vue'
import MultiSelect from './components/MultiSelect.vue'
import MultilineTextTable from './components/MultilineTextTable.vue'
import SearchableTable from './components/SearchableTable.vue'

const options = [
  { label: 'Option 1', value: 'option1' },
  { label: 'Option 2', value: 'option2' },
  { label: 'Option 3', value: 'option3' },
  { label: 'Option 4', value: 'option4' },
  { label: 'Option 5', value: 'option5' },
  { label: 'Option 6', value: 'option6' },
  { label: 'Option 7', value: 'option7' },
  { label: 'Option 8', value: 'option8' }
];
</script>

<template>
  <div class="row">
    <h2>Поле Link</h2>
    <LinkField />
  </div>
  <div class="row">
    <h2>DateTime picker</h2>
    <TimePicker />
  </div>
  <div class="row">
    <h2>Сброс значений в Select</h2>
    <MultiSelect :options="options" />
  </div>
  <div class="row">
    <h2>Многострочный текст в ячейке</h2>
    <MultilineTextTable />
  </div>
  <div class="row">
    <h2>Таблица с поиском (расширенное задание)</h2>
    <SearchableTable />
  </div>
</template>

<style scoped lang="scss">
  .row {
    margin-bottom: 30px;
  }
</style>