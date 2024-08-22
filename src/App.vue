<template>
  <div id="app">
    <h1>Поле Link</h1>
    <LinkInput />
    <h1>DateTime picker </h1>
    <TimePicker />
    <h1>Сброс значений в Select </h1>
    <MultiSelectReset />
    <h1>Многострочный текст в ячейке</h1>
    <MultiLineTextTable />
    <h1>Таблица с поиском</h1>
    <SearchableTable />
  </div>
</template>

<script lang="ts">
import LinkInput from './components/LinkInput.vue';
import TimePicker from './components/TimePicker.vue';
import MultiSelectReset from './components/MultiSelectReset.vue';
import MultiLineTextTable from './components/MultiLineTextTable.vue';
import SearchableTable from './components/SearchableTable.vue';

export default {
  name: 'App',
  components: {
    LinkInput,
    TimePicker,
    MultiSelectReset,
    MultiLineTextTable,
    SearchableTable,
  },
};
</script>

<style>
#app {
  text-align: center;
  margin-top: 50px;
}
</style>
