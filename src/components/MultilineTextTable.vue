<template>
  <div>
    <DataTable :value="data">
      <Column field="text">
        <template #body="{ data }">
          <div v-for="(line, index) in formatText(data.text)" :key="index" class="multiline-text">
            {{ line }}
          </div>
        </template>
      </Column>
    </DataTable>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

interface RowData {
  text: string;
}

const rawText = `[13:36:53] Расчетное время: 9 мин[13:36:58] Открыть клапан откачки К1[13:36:58] Включить вакуумный насос[13:36:58] Закрыть клапан К5[13:36:58] Закрыть клапан дистилляции К2[13:36:58] Ожидание: 8 с[13:37:06] Заливка 2.2мл. в испаритель[13:37:06] Заданно 26.50602409638554 шагов[13:37:09] Заливка перекиси завершена[13:37:09] Открыть клапан дистилляции К2[13:42:09] Включить нагрев испарителя[13:42:09] Закрыть клапан дистилляции К2[13:42:09] конечное давление1.0960040758227925 торр[13:42:09] Выпаривание через К2[13:42:09] Выпаривание длилось5 мин[13:42:09] Откачка до 1 торр[13:42:15] Закрыть клапан откачки К1[13:43:09] Открыть клапан откачки К1[13:43:14] Аппаратное смещение 0 денсит. = -0.313683180809021[13:43:14] Закрыть клапан дистилляции К2`;

function formatText(text: string): string[] {
  return text.split(/(?=\[\d{2}:\d{2}:\d{2}\])/).filter(line => line.trim() !== '');
}

const data = ref<RowData[]>([
  { text: rawText }
]);
</script>

<style scoped lang="scss">
.multiline-text {
  white-space: pre-line;
}
</style>
