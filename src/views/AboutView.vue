<template>
  <div>
    <h1>{{ pageTitle }}</h1>
    <button @click="fetchData" :disabled="loading">Fetch Data</button>
    <div v-if="loading" class="spinner-border text-primary" role="status">
      <span class="sr-only">Loading...</span>
    </div>
    <table v-if="!loading" class="table">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Username</th>
          <th scope="col">Email</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in users" :key="index">
          <td>{{ user.id }}</td>
          <td>{{ user.username }}</td>
          <td>{{ user.email }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Teachers',
  data() {
    return {
      pageTitle: 'Teachers',
      users: [], // Data will be fetched from Google Apps Script
      loading: false
    }
  },
  mounted() {
    // Fetch data on component mount
    this.fetchData();
  },
  methods: {
    fetchData() {
      // Set loading to true while fetching data
      this.loading = true;
      // Call Google Apps Script API to fetch data
      fetch('https://script.google.com/macros/s/AKfycbz1zheFO13LCjPBs5HO7xjOu2E3tMjkMYiH_tWWLoT285DB-Ie_44EODrIP1cgElGdm/exec?action=getUsersData')
        .then(response => response.json())
        .then(data => {
          // Check if data contains 'data' property
          if (data.hasOwnProperty('data')) {
            // Assign users data to the component's data
            this.users = data.data;
          }
        })
        .catch(error => console.error('Error fetching data:', error))
        .finally(() => {
          // Set loading to false after data is fetched
          this.loading = false;
        });
    }
  }
}
</script>

<style scoped>
/* Add component-specific styles here */
</style>
