<template>
  <div>
    <el-button
      type="success"
      style="float: left; margin-bottom: 10px"
      @click="showForm = true"
      >Sort</el-button
    >
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column label="No." width="80" align="center">
        <template slot-scope="{ $index }">
          <span>{{ $index + 1 }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="date" label="Date" width="180"> </el-table-column>
      <el-table-column prop="name" label="Name" width="180"> </el-table-column>
      <el-table-column prop="address" label="Address"> </el-table-column>
    </el-table>
    <el-dialog v-if="showForm" title="Sort Form" :visible.sync="showForm">
      <div>
        <el-checkbox-group v-model="checkboxGroup1">
          <el-checkbox-button v-for="name in names" :label="name" :key="name">{{
            name
          }}</el-checkbox-button>
        </el-checkbox-group>
        <span>Tuyến đường hiện tại: {{ names.join(" -> ") }}</span>
        <br />
        <span>Tuyến đường thay đổi: {{ checkboxGroup1.join(" -> ") }}</span>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="showForm = false">Cancel</el-button>
        <el-button type="primary" @click="sortTable">Sort</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "my-table",
  data() {
    return {
      showForm: false,
      tableData: [
        {
          date: "2016-05-03",
          name: "Circle K",
          address: "No. 189, Grove St, Los Angeles",
        },
        {
          date: "2016-05-02",
          name: "Starbuck",
          address: "No. 189, Grove St, Los Angeles",
        },
        {
          date: "2016-05-04",
          name: "Charging Station",
          address: "No. 189, Grove St, Los Angeles",
        },
        {
          date: "2016-05-01",
          name: "Phenikaa-X",
          address: "No. 189, Grove St, Los Angeles",
        },
      ],
      checkboxGroup1: [],
    };
  },
  computed: {
    names() {
      return this.tableData.map((item) => item.name);
    },
  },
  methods: {
    sortTable() {
      const filteredData = this.tableData.filter((item) =>
        this.checkboxGroup1.includes(item.name)
      );
      filteredData.sort((a, b) => {
        return (
          this.checkboxGroup1.indexOf(a.name) -
          this.checkboxGroup1.indexOf(b.name)
        );
      });
      this.tableData = filteredData.concat(
        this.tableData.filter(
          (item) => !this.checkboxGroup1.includes(item.name)
        )
      );
      this.showForm = false;
      this.checkboxGroup1 = [];
      this.$notify({
        title: "Success",
        message: "Changing route successfully!",
        type: "success",
      });
    },
  },
};
</script>

<style></style>
