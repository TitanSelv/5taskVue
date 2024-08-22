<template>
    <div>
        <p>Выбранное время: {{ formattedTime }}</p>
        <Calendar v-model="time" timeOnly hourFormat="24" :showIcon="true" placeholder="Выберите время" />
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
import Calendar from 'primevue/calendar';

export default defineComponent({
    name: 'TimePicker',
    components: {
        Calendar,
    },
    setup() {
        const time = ref<Date | null>(null); 

        const formattedTime = computed(() => {
            if (!time.value) return '';
            const hours = String(time.value.getHours()).padStart(2, '0');
            const minutes = String(time.value.getMinutes()).padStart(2, '0');
            return `${hours}:${minutes}`;
        });

        return {
            time,
            formattedTime,
        };
    },
});
</script>

<style scoped>
</style>