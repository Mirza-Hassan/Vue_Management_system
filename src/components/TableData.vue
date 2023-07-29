<template>
  <!-- Add the search input field -->
  <input v-model="searchText" class="custom-search-input" placeholder="Search by name" />

  <!-- Create a table using the 'a-table' component and bind it to the columns and filteredData properties -->
  <a-table :columns="columns" :data-source="filteredData" row-key="id" :row-selection="rowSelection" />
</template>

<script>
import { Table } from 'ant-design-vue';
import studentsData from './students_data.json'; // Import the JSON file

export default {
  components: {
    'a-table': Table,
  },
  data() {
    return {
      tableData: [],       // Store the table data and column definitions
      columns: [
        {
          // Column for 'ID' field
          title: 'ID',
          dataIndex: 'id',
          key: 'id',
          // Use the `customCell` property to apply styles to the "ID" column
          customCell: () => {
            return {
              style: {
                color: 'blue',
                textDecoration: 'underline',
              },
            };
          },
        },
        {
          // Column for 'Name' field
          title: 'Name',
          dataIndex: 'name',
          key: 'name',
        },
        {
          // Column for 'Gender' field
          title: 'Gender',
          dataIndex: 'gender',
          key: 'gender',
          // Use the `customCell` property to apply custom styles based on the 'gender' value
          customCell: (record) => {
            const genderColorMap = {
              female: 'pink',
              male: 'blue',
              diverse: 'grey',
            };

            return {
              style: {
                color: genderColorMap[record.gender.toLowerCase()],
              },
            };
          },
        },
        {
          // Column for 'Email & Telephone' field
          title: 'Email & Telephone',
          key: 'emailAndTelephone',
          // Use the `customRender` function to combine email and telephone
          customRender: ({ record }) =>
          `${record.email}  /  ${record.telephone}`,
        },
      ],
      selectedRowKeys: [], // Array to store the selected row keys
      searchText: '', // Search input value
    };
  },
  mounted() {
    // Assign the imported data to the tableData variable
    this.tableData = studentsData;
  },
  computed: {
    // Compute filteredData based on the search input value (this.searchText)
    filteredData() {
      return this.tableData.filter((item) =>
        item.name.toLowerCase().includes(this.searchText.toLowerCase())
      );
    },
    // Define the rowSelection object to enable "Select All" functionality
    rowSelection() {
      return {
        selectedRowKeys: this.selectedRowKeys,
        onChange: this.onSelectChange,
        selections: [
          {
            key: 'selectAll',
            text: 'Select All',
            onSelect: this.onSelectAllChange,
          },
          {
            key: 'selectInvert',
            text: 'Select Invert',
            onSelect: this.onSelectInvertChange,
          },
        ],
      };
    },
    },
    methods: {
    // Handle the change in selected row keys
    onSelectChange(selectedRowKeys) {
      this.selectedRowKeys = selectedRowKeys;
    },
    // Select all rows when the "Select All" option is chosen
    onSelectAllChange() {
      this.selectedRowKeys = this.tableData.map((row) => row.id);
    },
    // Invert the selection when the "Select Invert" option is chosen
    onSelectInvertChange() {
      this.selectedRowKeys = this.tableData
        .filter((row) => !this.selectedRowKeys.includes(row.id))
        .map((row) => row.id);
    },
  },
};
</script>


<style>

/* Custom styling for the search input field and its container */
.custom-search-input {
  padding: 8px 12px;
  margin-bottom: 20px;
  border: 2px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease;
  width: 300px;
}

</style>
