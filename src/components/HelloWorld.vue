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
      <el-table-column prop="date" label="Date" width="100"> </el-table-column>
      <el-table-column prop="name" label="Name" width="150"> </el-table-column>
      <el-table-column prop="status" label="Status" width="150">
      </el-table-column>
      <el-table-column prop="address" label="Address"> </el-table-column>
    </el-table>

    <el-dialog v-if="showForm" title="Sort Form" :visible.sync="showForm">
      <div style="text-align: left; margin-bottom: 5px">
        <span>
          <i class="square-icon-re"></i>
          Realesed Station
        </span>
        <span>
          <i class="square-icon-dr"></i>
          Drafted Station
        </span>
      </div>
      <div>
        <div class="button-container">
          <template v-for="(item, index) in nameAndStatus">
            <el-button
              v-if="item.show"
              :key="index"
              v-bind:type="item.status === 'Realese' ? 'primary' : 'danger'"
              @click="addToChooseName(item)"
            >
              {{ item.name }}
            </el-button>
          </template>
        </div>

        <el-steps direction="vertical" :active="1">
          <el-step
            v-for="(item, index) in chooseName"
            :key="index"
            :title="item"
          ></el-step>
        </el-steps>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="cancelSort" type="danger">Cancel</el-button>
        <el-button type="success" @click="sortTable">Sort</el-button>
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
      chooseName: [],
      tableData: [
        {
          date: "2016-05-03",
          name: "Circle K",
          address: "No. 189, Grove St, Los Angeles",
          status: "Realese",
        },
        {
          date: "2016-05-02",
          name: "Starbuck",
          address: "No. 189, Grove St, Los Angeles",
          status: "Realese",
        },
        {
          date: "2016-05-04",
          name: "Charging Station",
          address: "No. 189, Grove St, Los Angeles",
          status: "Realese",
        },
        {
          date: "2016-05-01",
          name: "Phenikaa-X",
          address: "No. 189, Grove St, Los Angeles",
          status: "Realese",
        },
        {
          date: "2016-05-03",
          name: "Vincom Center",
          address: "No. 189, Grove St, Los Angeles",
          status: "Draft",
        },
        {
          date: "2016-05-02",
          name: "Highland Coffee",
          address: "No. 189, Grove St, Los Angeles",
          status: "Draft",
        },
        {
          date: "2016-05-04",
          name: "Phenikaa School",
          address: "No. 189, Grove St, Los Angeles",
          status: "Draft",
        },
        {
          date: "2016-05-01",
          name: "Phenikaa Hospital",
          address: "No. 189, Grove St, Los Angeles",
          status: "Draft",
        },
      ],
    };
  },
  computed: {
    nameAndStatus() {
      return this.tableData.map(({ name, status }) => ({
        name,
        status,
        show: true,
      }));
    },
  },
  methods: {
    addToChooseName(item) {
      this.chooseName.push(item.name);
      item.show = false;
    },
    cancelSort() {
      this.nameAndStatus.forEach((item) => {
        item.show = true;
      });
      this.chooseName = [];
      this.showForm = false;
    },
    sortTable() {
      const sortedData = [];
      for (const name of this.chooseName) {
        const index = this.tableData.findIndex((item) => item.name === name);
        if (index !== -1) {
          sortedData.push(this.tableData[index]);
        }
      }
      this.tableData = sortedData;
      this.chooseName = [];
      this.showForm = false;
      this.$notify({
        title: "Success",
        message: "Changing route successfully!",
        type: "success",
      });
    },
  },
};
</script>

<style>
@media (max-width: 768px) {
  .el-dialog__wrapper {
    width: 100% !important;
    height: 100% !important;
    top: 0 !important;
    left: 0 !important;
    margin: 0 !important;
    border-radius: 0 !important;
  }
  .el-dialog {
    width: 100% !important;
    height: 100% !important;
    margin: 0 !important;
    border-radius: 0 !important;
  }
  .el-dialog__body {
    height: calc(100% - 65px) !important;
    overflow-y: scroll !important;
  }
}
.el-dialog__body .button-container {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
}

.el-dialog__body .button-container button {
  margin: 5px 0;
  width: 50%;
  text-align: left;
}
.square-icon-re {
  display: inline-block;
  width: 8px;
  height: 8px;
  margin-right: 5px;
  background-color: #3c8dbc;
}
.square-icon-dr {
  display: inline-block;
  width: 8px;
  height: 8px;
  margin-right: 5px;
  background-color: #e11818;
}
</style>
