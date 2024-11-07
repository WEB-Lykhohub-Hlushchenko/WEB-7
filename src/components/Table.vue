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
      localData: [...this.data], // локальна копія даних
      selectedRows: [],
      selectAll: false
    };
  },
  watch: {
    data(newData) {
      // Оновлення локальної копії, якщо змінилась prop data
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
        ...this.localData[index] // копіюємо дані рядка
      }));
      this.localData = [...this.localData, ...duplicatedRows]; // додаємо копії в локальні дані
      this.$emit('update:data', this.localData); // передаємо оновлені дані батьківському компоненту
      this.selectedRows = []; // очищаємо вибір
    },
    deleteSelectedRows() {
      this.localData = this.localData.filter((_, index) => !this.selectedRows.includes(index));
      this.$emit('update:data', this.localData); // передаємо оновлений масив даних батьківському компоненту
      this.selectedRows = []; // очищаємо вибір
    }
  }
};
</script>

<style scoped>
.table-container {
  margin-top: 20px;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
  margin-top: 10px;
}

th, td {
  padding: 8px;
  text-align: left;
  border: 1px solid #ddd;
}

th {
  background-color: #f4f4f4;
  color: #333;
}

.selected-row {
  background-color: #e0f7fa;
}

input[type="checkbox"] {
  margin: 0;
}

.action-buttons {
  display: flex;
  gap: 10px;
  justify-content: flex-start;
  margin-top: 15px;
}

.action-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.duplicate {
  background-color: #4CAF50;
  color: white;
}

.delete {
  background-color: #f44336;
  color: white;
}

.action-button:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}
</style>
