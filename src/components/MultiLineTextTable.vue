<template>
    <div class="table-container" :class="{ collapsed: isCollapsed }">
        <button @click="toggleCollapse" class="toggle-button">
            {{ isCollapsed ? 'Развернуть таблицу' : 'Свернуть таблицу' }}
        </button>
        <div v-if="!isCollapsed" class="table-content">
            <DataTable :value="rows" class="small-table">
                <Column field="timestamp" header="Время"></Column>
                <Column field="event" header="Событие" :body="formatEvent"></Column>
            </DataTable>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

export default defineComponent({
    name: 'MultiLineTextTable',
    components: {
        DataTable,
        Column,
    },
    setup() {
        const apiString = `[13:36:53] Расчетное время: 9 мин[13:36:58] Открыть клапан откачки К1[13:36:58] Включить вакуумный насос[13:36:58] Закрыть клапан К5[13:36:58] Закрыть клапан дистилляции К2[13:36:58] Ожидание: 8 с[13:37:06] Заливка 2.2мл. в испаритель[13:37:06] Заданно 26.50602409638554 шагов[13:37:09] Заливка перекиси завершена[13:37:09] Открыть клапан дистилляции К2[13:42:09] Включить нагрев испарителя[13:42:09] Закрыть клапан дистилляции К2[13:42:09] конечное давление1.0960040758227925 торр[13:42:09] Выпаривание через К2[13:42:09] Выпаривание длилось5 мин[13:42:09] Откачка до 1 торр[13:42:15] Закрыть клапан откачки К1[13:43:09] Открыть клапан откачки К1[13:43:14] Аппаратное смещение 0 денсит. = -0.313683180809021[13:43:14] Закрыть клапан дистилляции К2`;
        const rows = ref(apiString.split(/(?=\[\d{2}:\d{2}:\d{2}\])/).map(event => {
            const match = event.match(/(\[\d{2}:\d{2}:\d{2}\])(.+)/);
            return {
                timestamp: match ? match[1] : '',
                event: match ? match[2].trim() : '',
            };
        }));

        const formatEvent = (rowData: { event: string }) => {
            return rowData.event.split('[').join('<br/>');
        };
        const isCollapsed = ref(false);
        
        const toggleCollapse = () => {
            isCollapsed.value = !isCollapsed.value;
        };
        return {
            rows,
            formatEvent,
            isCollapsed,
            toggleCollapse,
        };
    },
});
</script>

<style scoped>
.table-container {
    width: 50%;
    margin: 0 auto;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.table-content {
    max-height: 300px;
    overflow-y: auto;
}

.small-table .p-datatable {
    font-size: 0.9em;
}

.toggle-button {
    display: block;
    margin-bottom: 10px;
    padding: 5px 10px;
    border: none;
    background-color: #007ad9;
    color: white;
    border-radius: 4px;
    cursor: pointer;
}

.toggle-button:hover {
    background-color: #005ea1;
}

.collapsed .table-content {
    display: none;
}
</style>