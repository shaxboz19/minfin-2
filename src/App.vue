

<template>
  <div>
    <AppModal
      :isModalOpen="isModalOpen"
      :value="columns[activeIndex]?.userCode"
      @close="closeModal"
      @save="updateColumns"
    />
    <AppTable :data="data" :columns="columns" @openModal="openModal" />
  </div>
</template>

<script>
import AppTable from './components/AppTable.vue';
import AppModal from './components/AppModal.vue';

export default {
  components: {
    AppTable,
    AppModal,
  },
  data() {
    return {
      data: [
        {
          id: 1,
          name: 'John',
          age: 20,
          city: 'New York',
        },
        {
          id: 2,
          name: 'Jane',
          age: 24,
          city: 'Paris',
        },
        {
          id: 3,
          name: 'Bob',
          age: 18,
          city: 'Tokyo',
        },
        {
          id: 4,
          name: 'Kate',
          age: 21,
          city: 'Moscow',
        },
        {
          id: 5,
          name: 'Alex',
          age: 30,
          city: 'Berlin',
        },
      ],
      columns: [
        {
          field: 'id',
          title: 'ID',
        },
        {
          field: 'name',
          title: 'Name',
        },
        {
          field: 'age',
          title: 'Age',
        },
        {
          field: 'city',
          title: 'City',
        },
      ],
      isModalOpen: false,
      activeIndex: null,
    };
  },
  methods: {
    openModal(index) {
      this.activeIndex = index;
      this.isModalOpen = true;
    },
    closeModal() {
      this.activeIndex = null;
      this.isModalOpen = false;
    },
    updateColumns(userCode) {
      const index = this.activeIndex;
      this.columns[index].userCode = userCode;
      this.columns[index].renderFn = (item) =>
        userCode.replace('{value}', item[this.columns[index].field]);
      this.closeModal();
    },
  },
};
</script>


