<template>
    <div>
        <InputText v-if="isEditing || !url" v-model="url" placeholder="https://" @keydown.enter="fetchTitle"
            @blur="handleBlur" />

        <div v-else>
            <a :href="url" target="_blank">{{ title || url }}</a>
            <i v-if="url" class="pi pi-pencil" @click="isEditing = true" style="cursor: pointer; margin-left: 8px;"></i>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import axios from 'axios';

export default defineComponent({
    name: 'LinkInput',
    setup() {
        const url = ref<string>('');
        const title = ref<string>('');         
        const isEditing = ref<boolean>(true);  

        const fetchTitle = async () => {
            if (url.value.trim() === '') {
                title.value = '';
                isEditing.value = true;
                return;
            }

            isEditing.value = false;
            try {
                const response = await axios.get(`https://api.allorigins.win/get?url=${encodeURIComponent(url.value)}`);
                const parser = new DOMParser();
                const doc = parser.parseFromString(response.data.contents, 'text/html');
                title.value = doc.querySelector('title')?.innerText || url.value;
            } catch (error) {
                console.error('Ошибка при получении title:', error);
                title.value = url.value; 
            }
        };
        const handleBlur = () => {
            if (url.value.trim() === '') {
                isEditing.value = true;
            } else {
                fetchTitle();
            }
        };

        return {
            url,
            title,
            isEditing,
            fetchTitle,
            handleBlur,
        };
    },
});
</script>

<style scoped>

</style>