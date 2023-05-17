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
    <!-- route dialog -->
    <el-dialog
      title="Route Dialog"
      :visible.sync="showForm"
      :fullscreen="isMobile"
    >
      <!-- change station dialog -->
      <el-dialog
        title="Station Dialog"
        :visible.sync="showStationForm"
        :fullscreen="isMobile"
        append-to-body
        width="40%"
      >
        <span>Current station</span>
        <el-input
          :value="chooseStation.name"
          readonly
          style="margin: 10px 0px 10px 0px"
        ></el-input>
        <span>Choose new station</span>
        <el-select
          v-model="value"
          placeholder="Select"
          style="width: 100%; margin: 10px 0px 10px 0px"
          @change="handleSelectChange"
        >
          <el-option
            v-for="(item, index) in filteredData"
            :key="index"
            :label="item.name"
            :value="item.name"
          >
          </el-option>
        </el-select>
        <span slot="footer" class="dialog-footer">
          <el-button @click="cancelChange" type="danger">Cancel</el-button>
          <el-button @click="confirmChange" type="success">Confirm</el-button>
        </span>
      </el-dialog>
      <template v-for="(item, index) in filteredData">
        <el-row :key="index" style="margin-bottom: 10px">
          <el-col :span="4">
            <i :key="index" class="el-icon-location-outline"></i>
          </el-col>
          <el-col :span="16">
            <el-input :key="index" :value="item.name" readonly>
              <template #append>
                <el-button
                  type="primary"
                  icon="el-icon-more-outline"
                  @click="changeStation(item)"
                ></el-button>
              </template>
            </el-input>
          </el-col>
          <el-col :span="4">
            <el-button
              icon="el-icon-circle-close"
              circle
              @click="deleteStation(item)"
            ></el-button>
          </el-col>
        </el-row>
      </template>
      <div>
        <el-row>
          <el-col :span="4">
            <i class="el-icon-add-location"></i>
          </el-col>
          <el-col :span="16">
            <el-input value="Add a new station" readonly v-if="!addOn">
              <template #append>
                <el-button
                  type="primary"
                  icon="el-icon-circle-plus-outline"
                  @click="addStationOn"
                ></el-button>
              </template>
            </el-input>
            <div v-else>
              <el-select
                v-model="addValue"
                placeholder="Select A Station"
                style="width: 100%"
                @change="addStation"
              >
                <el-option
                  v-for="(item, index) in filteredData"
                  :key="index"
                  :label="item.name"
                  :value="item.name"
                >
                </el-option>
              </el-select>
            </div>
          </el-col>
          <el-col :span="4">
            <el-button
              v-if="addOn"
              icon="el-icon-circle-close"
              circle
              @click="cancelAdd"
            ></el-button>
          </el-col>
        </el-row>
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
      showStationForm: false,
      chooseStation: [],
      selectedValue: [],
      addValue: "",
      isMobile: false,
      addOn: false,
      value: "",
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
  mounted() {
    this.checkMobileScreen();
    window.addEventListener("resize", this.checkMobileScreen);
  },
  computed: {
    filteredData() {
      return this.tableData.filter((item) => item.status === "Realese");
    },
  },
  methods: {
    addStation() {
      const addStation = this.tableData.find(
        (item) => item.name === this.addValue
      );
      if (addStation) {
        this.filteredData.push(addStation);
      }
      this.addValue = "";
      this.addOn = false;
    },
    deleteStation(item) {
      const index = this.filteredData.indexOf(item);
      if (index !== -1) {
        this.filteredData.splice(index, 1);
      }
    },
    cancelAdd() {
      this.addOn = false;
    },
    addStationOn() {
      this.addOn = true;
    },
    handleSelectChange() {
      this.selectedValue = this.value;
    },
    confirmChange() {
      console.log(this.selectedValue);
    },
    changeStation(item) {
      this.showStationForm = true;
      this.chooseStation = item;
    },
    cancelChange() {
      this.showStationForm = false;
      this.value = [];
    },
    checkMobileScreen() {
      this.isMobile = window.innerWidth <= 768; // Thay đổi 768 thành kích thước màn hình phù hợp cho thiết bị di động của bạn
    },
    cancelSort() {
      this.showForm = false;
    },
    sortTable() {
      this.showForm = false;
    },
  },
};
</script>

<style></style>
