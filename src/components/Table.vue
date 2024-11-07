<template>
  <div class="table-container">
    <table>
      <thead>
      <tr>
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
      <tr v-for="(row, index) in data" :key="index">
        <td>{{ row.email }}</td>
        <td>{{ row.firstName }}</td>
        <td>{{ row.lastName }}</td>
        <td>{{ row.middleName }}</td>
        <td>{{ row.phone }}</td>
        <td>{{ row.birthDate }}</td>
        <td>{{ row.gender }}</td>
        <td>{{ row.role }}</td>
        <td>
          <button @click="$emit('duplicate', index)">Дублювати</button>
          <button @click="$emit('delete', index)">Видалити</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    data: Array
  }
}
</script>

<style>
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

.action-button {
  padding: 5px 10px;
  margin-right: 5px;
  border: none;
  border-radius: 4px;
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
</style>
