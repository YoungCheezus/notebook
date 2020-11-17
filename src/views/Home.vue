<template>
  <div class="content">
    <el-row>
      <el-col :span="20">
        <h2>My contacts</h2>
      </el-col>
      <el-col :span="4">
        <div class="right">
          <el-button v-if="showBtn" size="medium" @click="handleEdit()" type="warning">
            Edit
          </el-button>
          <el-button v-else size="medium" @click="handleSave()" type="success">
            Save
          </el-button>
        </div>
      </el-col>
    </el-row>

    <hr />
    <div class="margin-3"></div>
    <!-- Adaptive "input"  -->
    <el-input v-model="search" size="mini" class="search-xs" placeholder="Search by name"/>
    <!-- Create table , input data, search filter by "name"-->
    <el-table :data="contacts.data.filter((data) => !search || data.name.toLowerCase().includes(search.toLowerCase()))" style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props">
          <!-- Table data, expandable row -->
          <p v-if="!isEdit">State: {{ props.row.address.state }}</p>
          <!-- Input. Edit Contacts data -->
          <template v-else slot-scope="props"> State:
            <el-input v-if="isEdit" v-model="props.row.address.state" size="mini"></el-input>
            <div class="margin-1"></div>
          </template>
          <!-- Table data, expandable row -->
          <p v-if="!isEdit">City: {{ props.row.address.city }}</p>
          <template v-else slot-scope="props"> City: 
            <!-- Input. Edit Contacts data -->
            <el-input v-if="isEdit" v-model="props.row.address.city" size="mini"></el-input>
            <div class="margin-1"></div>
          </template>
          <!-- Table data, expandable row -->
          <p v-if="!isEdit">Address: {{ props.row.address.streetC }}</p>
          <template v-else slot-scope="props"> Address:
            <!-- Input. Edit Contacts data -->
            <el-input v-if="isEdit" v-model="props.row.address.streetC" size="mini"></el-input>
            <div class="margin-1"></div>
          </template>
            <!-- Table data, expandable row -->
          <p v-if="!isEdit">Zip: {{ props.row.address.zipcode }}</p>
          <template v-else slot-scope="props"> Zip:
            <!-- Input. Edit Contacts data -->
            <el-input v-if="isEdit" v-model="props.row.address.zipcode" size="mini"></el-input>
            <div class="margin-1"></div>
          </template>
        </template>
      </el-table-column>
      <!-- Table data, column -->
      <el-table-column label="Name" prop="name" sortable>
        <template slot-scope="props">
          <p v-if="!isEdit">{{ props.row.name }}</p>
          <template v-else v-show="isEdit" slot-scope="props">
            <!-- Input. Edit Contacts data -->
            <el-input v-model="props.row.name"></el-input>
          </template>
        </template>
      </el-table-column>
      <!-- Table data, column -->
      <el-table-column label="Phone" prop="phone">
        <template slot-scope="props">
          <p v-if="!isEdit">{{ props.row.phone }}</p>
          <template v-else v-show="isEdit" slot-scope="props">
            <!-- Input. Edit Contacts data -->
            <el-input v-model="props.row.phone"></el-input>
          </template>
        </template>
      </el-table-column>
      <!-- Table data, column -->
      <el-table-column label="E-mail" prop="email" width="280">
        <template slot-scope="props">
          <p v-if="!isEdit">{{ props.row.email }}</p>
          <template v-else v-show="isEdit" slot-scope="props">
            <!-- Input. Edit Contacts data -->
            <el-input v-model="props.row.email"></el-input>
          </template>
        </template>
      </el-table-column>
      <!-- Search input -->
      <el-table-column align="right">
        <template slot="header" slot-scope="scope">
          <el-input
            v-model="search"
            size="mini"
            class="visible-xs"
            placeholder="Search by name"
          />
        </template>
        <!--  column for buttons "Remove"-->
        <template slot-scope="scope">
          <el-button
            @click.native.prevent="deleteRow(scope.$index, contacts)"
            type="danger"
            size="mini"
          >
            Remove
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      contacts: null, // table data
      search: "", // input search
      isEdit: false, // show || !show edit inputs
      showBtn: true, // show Edit button || show Save button
    };
  },
  methods: {
    
    handleEdit() {
      this.isEdit = true;
      this.showBtn = !this.showBtn;
    },
    handleSave() {
      this.isEdit = false;
      this.saveContact();
      this.showBtn = !this.showBtn;
    },
    deleteRow(index, rows) {
      // delete row from "contacts"
      rows.data.splice(index, 1);
      this.saveContact();
    },
    saveContact() {
      // save changes in LocalStorage
      const parsed = JSON.stringify(this.contacts);
      localStorage.setItem("contacts", parsed);
    },
  },
  mounted() {
    // get data from "demo.sibers.com"
    if (localStorage.contacts == undefined) {
      axios
        .get("http://demo.sibers.com/users")
        .then((response) => (this.contacts = response));
    } else {
    }
    // get data from localStorage
    if (localStorage.getItem("contacts")) {
      try {
        this.contacts = JSON.parse(localStorage.getItem("contacts"));
      } catch (e) {
        localStorage.removeItem("contacts");
      }
    }
  },
  watch: {
    // send data to localStorage
    contacts() {
      const parsed = JSON.stringify(this.contacts);
      localStorage.setItem("contacts", parsed);
    },
  },
};
</script>
