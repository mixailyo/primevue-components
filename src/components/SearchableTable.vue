<template>
  <div>
    <div class="search-controls">
      <InputGroup>
        <Button 
          icon="pi pi-sliders-h" 
          @click="toggleSearchFields" 
        />
        <div class="search-input-wrapper">
          <InputText 
            class="input"
            v-model="searchQuery" 
            placeholder="Поиск..." 
            @input="onInput" 
            @keyup.enter="performSearch"
          />
          <i 
            v-if="searchQuery" 
            class="pi pi-times clear-icon" 
            @click="clearSearch"
          ></i>
        </div>
        <Button label="Search" @click="performSearch" />
      </InputGroup>
      
      <MultiSelect 
        v-model="selectedColumns" 
        :options="columnOptions" 
        optionLabel="header" 
        placeholder="Поля таблицы" 
      />
    </div>

    <DataTable :value="filteredData" :responsiveLayout="'scroll'">
      <Column v-for="col in selectedColumns" :key="col.field" :field="col.field" :header="col.header" />
    </DataTable>

    <div v-if="filteredData.length === 0" class="no-data-message">
      Нет данных, соответствующих вашему запросу.
    </div>

    <Dialog 
      v-model:visible="dialogVisible" 
      header="Выберите поля для поиска" 
      :modal="true" 
      :style="{ width: '400px' }"
    >
      <MultiSelect 
        v-model="searchableColumns" 
        :options="columnOptions" 
        optionLabel="header" 
        placeholder="Выберите поля" 
        class="searchable-columns-select"
      />
      <Button 
        label="Закрыть" 
        @click="dialogVisible = false" 
        class="close-button"
      />
    </Dialog>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import MultiSelect from 'primevue/multiselect';
import InputGroup from 'primevue/inputgroup';
import Dialog from 'primevue/dialog';

interface Product {
  id: number;
  name: string;
  category: string;
  price: number;
}

const fieldOptions = [
  { field: 'name', header: 'Name' },
  { field: 'category', header: 'Category' },
  { field: 'price', header: 'Price' }
];

const data: Product[] = [
  { id: 1, name: 'Product 1', category: 'Category 1', price: 100 },
  { id: 2, name: 'Product 2', category: 'Category 2', price: 150 },
  { id: 3, name: 'Product 3', category: 'Category 1', price: 200 },
  { id: 4, name: 'Product 4', category: 'Category 3', price: 250 },
  { id: 5, name: 'Product 5', category: 'Category 2', price: 300 },
  { id: 6, name: 'Product 6', category: 'Category 1', price: 350 },
  { id: 7, name: 'Product 7', category: 'Category 3', price: 400 },
  { id: 8, name: 'Product 8', category: 'Category 2', price: 450 },
  { id: 9, name: 'Product 9', category: 'Category 3', price: 500 },
  { id: 10, name: 'Product 10', category: 'Category 1', price: 550 }
];

const searchQuery = ref<string>('');
const selectedColumns = ref<{ field: string, header: string }[]>(fieldOptions);

const columnOptions = ref(fieldOptions);
const searchableColumns = ref(fieldOptions);

const filteredData = ref<Product[]>(data);
const dialogVisible = ref<boolean>(false);

const onInput = () => {
  if (!searchQuery.value) clearSearch();
};

const clearSearch = () => {
  searchQuery.value = '';
  filteredData.value = data;
};

const performSearch = () => {
  const query = searchQuery.value.toLowerCase();
  
  if (!query) {
    filteredData.value = data;
    return;
  }

  filteredData.value = data.filter(item =>
    searchableColumns.value.some(col => 
      String(item[col.field as keyof Product]).toLowerCase().includes(query)
    )
  );
};

const toggleSearchFields = () => {
  dialogVisible.value = !dialogVisible.value;
};
</script>

<style scoped>
.search-controls {
  display: grid;
  align-items: center;
  gap: 15px;
  grid-template-columns: 4fr 1fr;
}

.search-input-wrapper {
  position: relative;
  width: 100%;
}

.input {
  width: 100%!important;
  border-radius: unset!important;
}

.clear-icon {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  cursor: pointer;
  color: #999;
  font-size: 1rem;
}

.clear-icon:hover {
  color: #333;
}

.searchable-columns-select {
  width: 100%;
}

.close-button {
  margin-top: 10px;
}

.no-data-message {
  text-align: center;
  margin-top: 20px;
  font-size: 1rem;
  color: #666;
}
</style>