<template>
    <div class="searchable-table-container">
        <div class="search-header">
            <div class="input-group">
                <InputText v-model="searchQuery" @keypress.enter="performSearch" placeholder="Поиск..."
                    class="search-input" />
                <Button icon="pi pi-search" class="p-button-rounded p-button-secondary search-button"
                    @click="performSearch" />
                <Button v-if="searchQuery" icon="pi pi-times" class="p-button-rounded p-button-secondary clear-btn"
                    @click="clearSearch" />
            </div>
            <div class="column-selector">
                <MultiSelect v-model="selectedColumns" :options="columnOptions" optionLabel="header"
                    placeholder="Поля таблицы" display="chip" class="column-multiselect" />
            </div>
        </div>
        <DataTable :value="filteredData" class="p-datatable-sm" :paginator="true" :rows="10"
            :rowsPerPageOptions="[10, 20, 50]" dataKey="id">
            <template v-for="col in selectedColumns" :key="col.field">
                <Column :field="col.field" :header="col.header" />
            </template>
        </DataTable>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import InputText from 'primevue/inputtext';
import MultiSelect from 'primevue/multiselect';
import Button from 'primevue/button';

export default defineComponent({
    name: 'SearchableTable',
    components: {
        DataTable,
        Column,
        InputText,
        MultiSelect,
        Button,
    },
    setup() {
        const data = ref([
            { id: 1, name: 'Холодильник', price: 100, category: 'Электроника' },
            { id: 2, name: 'Кроссовки', price: 150, category: 'Одежда' },
            { id: 3, name: 'Стол', price: 200, category: 'Дом' },
            { id: 4, name: 'Телефон', price: 250, category: 'Электроника' },
            { id: 5, name: 'Футболка', price: 300, category: 'Одежда' },
            { id: 6, name: 'ДИван', price: 350, category: 'Дом' },
            { id: 7, name: 'Планшет', price: 400, category: 'Электроника' },
            { id: 8, name: 'Худи', price: 450, category: 'Одежда' },
            { id: 9, name: 'Шкаф', price: 500, category: 'Дом' },
            { id: 10, name: 'Часы', price: 550, category: 'Электроника' },
            { id: 11, name: 'Шорты', price: 600, category: 'Одежда' },
        ]);

        const columnOptions = [
            { field: 'name', header: 'Название' },
            { field: 'price', header: 'Цена' },
            { field: 'category', header: 'Категория' },
        ];

        const selectedColumns = ref([...columnOptions]);
        const searchQuery = ref('');
        const searchFilter = ref('');

        const filteredData = computed(() => {
            if (!searchFilter.value) {
                return data.value;
            }
            return data.value.filter((row) =>
                selectedColumns.value.some((col) =>
                    String(row[col.field] || '').toLowerCase().includes(searchFilter.value.toLowerCase())
                )
            );
        });

        const clearSearch = () => {
            searchQuery.value = '';
            searchFilter.value = '';
        };

        const performSearch = () => {
            searchFilter.value = searchQuery.value;
        };

        return {
            data,
            columnOptions,
            selectedColumns,
            searchQuery,
            filteredData,
            clearSearch,
            performSearch,
        };
    },
});
</script>


<style scoped>
.searchable-table-container {
    width: 50%;
    margin: 0 auto;
    padding: 20px;
}

.search-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}

.input-group {
    display: flex;
    width: 100%;
    align-items: center;
}

.search-input {
    flex: 1;
    margin-right: 15px;
}

.search-button {
    margin-right: 15px;
}

.clear-btn {
    margin: 0 15px 0 0  ;
}

.column-selector {
    width: 20%;
}

.column-multiselect {
    width: 100%;
}

.p-datatable-sm {
    margin-top: 20px;
    
}
</style>
