<template>
    <div class="multi-select-reset">
        <MultiSelect v-model="selectedValues" :options="options" optionLabel="name" placeholder="Выберите опции"
            :display="selectedValues.length > 0 ? 'chip' : 'comma'" />

        <i v-if="selectedValues.length > 0" class="pi pi-times reset-button" @click="resetSelection"
            title="Сбросить выбор"></i>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import MultiSelect from 'primevue/multiselect';

export default defineComponent({
    name: 'MultiSelectReset',
    components: {
        MultiSelect,
    },
    setup() {
        const options = ref([
            { name: 'Option 1', code: 'O1' },
            { name: 'Option 2', code: 'O2' },
            { name: 'Option 3', code: 'O3' },
            { name: 'Option 4', code: 'O4' },
            { name: 'Option 5', code: 'O5' },
        ]);

        const selectedValues = ref<any[]>([]);

        const resetSelection = () => {
            selectedValues.value = [];
        };

        return {
            options,
            selectedValues,
            resetSelection,
        };
    },
});
</script>

<style scoped>
.multi-select-reset {
    position: relative;
    display: inline-block;
}

.reset-button {
    position: absolute;
    top: 50%;
    right: -25px;
    transform: translateY(-50%);
    cursor: pointer;
    font-size: 1.2em;
    color: #d32f2f;
}
</style>