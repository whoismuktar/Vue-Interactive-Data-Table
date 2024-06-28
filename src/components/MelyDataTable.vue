<template>
  <v-app>
    <v-container>
      <v-text-field v-model="search" label="Search" class="mb-4"></v-text-field>
      <v-data-table
        :headers="headers"
        :items="items"
        :search="search"
        item-key="email"
        show-select
        class="elevation-1"
        v-bind="tableOptions"
        @column-width-change="onColumnWidthChange"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Responsive Data Table</v-toolbar-title>
          </v-toolbar>
        </template>
      </v-data-table>
    </v-container>
  </v-app>
</template>

<script>
// import { VDataTableHeader } from 'vuetify/lib/components/VDataTable/VDataTableHeader'

export default {
  data: () => ({
    search: "",
    headers: [
      { text: "Name", value: "name", sortable: true },
      { text: "Email", value: "email", sortable: true },
      { text: "Phone Number", value: "phoneNumber", sortable: true },
      { text: "Lead Status", value: "role", sortable: true },
      { text: "Create Date (EDT)", value: "createDate", sortable: true },
    ],
    items: [
      {
        id: 1,
        name: "John Doe",
        phoneNumber: "45672343219",
        role: "Lead",
        email: "john.doe@example.com",
        age: 30,
        status: "Active",
        createDate: new Date(),
      },
      {
        id: 2,
        name: "Jane Smith",
        phoneNumber: "25672343219",
        role: "Lead",
        email: "jane.smith@example.com",
        age: 25,
        status: "Inactive",
        createDate: new Date(),
      },
      {
        id: 3,
        name: "Sam Johnson",
        phoneNumber: "15672343219",
        role: "Lead",
        email: "sam.johnson@example.com",
        age: 35,
        status: "Active",
        createDate: new Date(),
      },
      {
        id: 4,
        name: "Lisa Brown",
        phoneNumber: "75672343219",
        role: "Lead",
        email: "lisa.brown@example.com",
        age: 28,
        status: "Active",
        createDate: new Date(),
      },
      {
        id: 5,
        name: "Paul Davis",
        phoneNumber: "85672343219",
        role: "Lead",
        email: "paul.davis@example.com",
        age: 22,
        status: "Inactive",
        createDate: new Date(),
      },
      // Add more data as needed
    ],
    tableOptions: {
      draggable: true,
    },
  }),
  methods: {
    onColumnWidthChange(newWidth, oldWidth, header) {
      const updatedHeader = this.headers.find((h) => h.value === header.value);
      if (updatedHeader) {
        updatedHeader.width = newWidth;
      }
    },
  },
};
</script>

<style>
.v-data-table-header th {
  cursor: col-resize;
}
</style>
