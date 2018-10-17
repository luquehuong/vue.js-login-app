<template>
  <div class="container w-full mx-auto">
    <table>
      <thead>
        <tr>
          <th @click="sort('name')">Name</th>
          <th @click="sort('username')">Username</th>
          <th @click="sort('email')">Email</th>
          <th @click="sort('phone')">Phone</th>
          <th @click="sort('company')">Company</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in sortedList" :key="user.id">
          <td>{{ user.name }}</td>
          <td>{{ user.username }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.company.name }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data () {
    return {
      list: [],
      currentSort: '',
      currentSortDir: ''
    }
  },
  async created () {
    try {
      const { data } = await axios.get('https://jsonplaceholder.typicode.com/users')
      this.list = data
    } catch (e) {
      console.error(e)
    }
  },
  methods: {
    sort: function (s) {
      // if s == current sort, reverse
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
      }
      this.currentSort = s
    }
  },
  computed: {
    sortedList: function () {
      return this.list.slice(0).sort((a, b) => {
        let modifier = 1
        if (this.currentSortDir === 'desc') modifier = -1
        if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier
        if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier
        return 0
      })
    }
  }
}
</script>
<style scoped>
img {
  width: 20px;
  height: 70px;
}
table {
  font-family: 'Open Sans', sans-serif;
  width: 100%;
}
table th {
  width: 20%;
  padding: 1rem;
  text-transform: uppercase;
  text-align: center;
  background: #2980b9;
  color: #FFF;
  cursor: pointer;
}
table th:hover {
  background: #2472a5;
}
table td {
  padding: 0.25rem;
  text-align: center;
  border-right: 1px solid #afc5d3;
}
table td:last-child {
  border-right: none;
}
table tbody tr:nth-child(2n) td {
  background: #e5f1f9;
}
</style>
