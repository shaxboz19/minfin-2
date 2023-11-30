<template>
  <table class="custom-table">
    <thead>
      <tr>
        <th
          v-for="(column, index) in columns"
          :key="column.field"
          @dblclick="openModal(index)"
        >
          {{ column.title }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in data" :key="item.id">
        <td v-for="column in columns" :key="column.field">
          <div v-if="!column.renderFn">{{ item[column.field] }}</div>
          <div v-else v-html="cleanCode(column.renderFn(item))"></div>
        </td>
      </tr>
    </tbody>
  </table>
</template>
  
  <script>
import DOMPurify from 'isomorphic-dompurify';
export default {
  props: {
    data: {
      type: Array,
      required: true,
    },
    columns: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      userCode: '',
    };
  },

  methods: {
    openModal(column) {
      this.$emit('open-modal', column);
    },
    cleanCode(code) {
      return DOMPurify.sanitize(code);
    },
  },
};
</script>
  
  <style scoped>
.custom-table {
  border-collapse: collapse;
  width: 100%;
  text-align: left;
  border: 1px solid #ddd;
}
.custom-table th,
.custom-table td {
  padding: 8px;
  border: 1px solid #ddd;
}
</style>
  