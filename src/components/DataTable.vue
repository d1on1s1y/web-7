<template>
    <div class="table-container">
      <table id="usersTable">
        <thead>
        <tr>
          <th>
            <input type="checkbox" v-model="selectAll" @change="toggleSelectAll" />
          </th>
          <th>Емейл</th>
          <th>Ім'я</th>
          <th>Прізвище</th>
          <th>По батькові</th>
          <th>Номер телефону</th>
          <th>Дата народження</th>
          <th>Стать</th>
          <th>Роль</th>
        </tr>
        </thead>
        <tbody>
        <tr
            v-for="(row, index) in localData"
            :key="index"
            :class="{'selected-row': isRowSelected(index)}"
        >
          <td>
            <input type="checkbox" v-model="selectedRows" :value="index" class="row-select" />
          </td>
          <td>{{ row.email }}</td>
          <td>{{ row.firstName }}</td>
          <td>{{ row.lastName }}</td>
          <td>{{ row.middleName }}</td>
          <td>{{ row.phone }}</td>
          <td>{{ row.birthDate }}</td>
          <td>{{ row.gender }}</td>
          <td>{{ row.role }}</td>
        </tr>
        </tbody>
      </table>
  
      <div class="action-buttons">
        <button
            class="action-button duplicate"
            :disabled="!selectedRows.length"
            @click="duplicateSelectedRows"
        >
          Дублювати виділені
        </button>
        <button
            class="action-button delete"
            :disabled="!selectedRows.length"
            @click="deleteSelectedRows"
        >
          Видалити виділені
        </button>
      </div>
    </div>
  </template>

<script>


export default {
  props: {
    data: Array
  },
  data() {
    return {
      localData: [...this.data],
      selectedRows: [],
      selectAll: false
    };
  },
  watch: {
    data(newData) {
      this.localData = [...newData];
    },
    selectedRows(newSelectedRows) {
      this.selectAll = newSelectedRows.length === this.localData.length;
    },
    selectAll(newValue) {
      if (newValue) {
        this.selectedRows = this.localData.map((_, index) => index);
      } else {
        this.selectedRows = [];
      }
    }
  },
  methods: {
    isRowSelected(index) {
      return this.selectedRows.includes(index);
    },
    toggleSelectAll() {
      if (this.selectAll) {
        this.selectedRows = this.localData.map((_, index) => index);
      } else {
        this.selectedRows = [];
      }
    },
    duplicateSelectedRows() {
      const duplicatedRows = this.selectedRows.map(index => ({
        ...this.localData[index]
      }));
      this.localData = [...this.localData, ...duplicatedRows];
      this.$emit('duplicate', this.localData); 
      this.selectedRows = []; 
    },
    deleteSelectedRows() {
      this.localData = [...this.localData.filter((_, index) => !this.selectedRows.includes(index))];
      this.$emit('delete', this.localData); 
      this.selectedRows = [];
    }
  }
};
</script>