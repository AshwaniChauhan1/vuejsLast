<template>
  <div>
    <div class="jumbotron">
      <h1>FORM - TABLE - ASSIGNMENT</h1>
    </div>
    <div class="text-left pl-5">
      <b-button v-b-modal.modal-1>Add Details</b-button>
    </div>
    <b-modal id="modal-1" ref="my-modal" hide-footer @hide="clear">
      <template v-slot:modal-header>
        <h4>Fill Form</h4>
      </template>
      <template v-slot>
        <b-form v-on:submit.prevent>
          <b-form-group class="text-left" label="Email Address:">
            <b-form-input type="email" placeholder="Enter email" v-model="form.email" required></b-form-input>
          </b-form-group>
          <b-form-group class="text-left" label="Name:">
            <b-form-input type="text" placeholder="Enter Name" v-model="form.name" required></b-form-input>
          </b-form-group>
          <b-form-group class="text-left" label="DOB:">
            <b-form-input type="date" required v-model="form.dob"></b-form-input>
          </b-form-group>
          <b-form-group class="text-left" label="Password:">
            <b-form-input type="password" placeholder="Enter Password" v-model="password" required></b-form-input>
          </b-form-group>
          <b-form-group class="text-left" label="Confirm Password:">
            <b-form-input
              type="password"
              placeholder="Enter Confirm Password"
              v-model="cpassword"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-checkbox
            name="checkbox-1"
            value="accepted"
            unchecked-value="not_accepted"
            v-model="status"
          >I accept the terms and conditions</b-form-checkbox>
          <p id="error">{{error}}</p>
          <div class="justify-content-between d-flex py-3">
            <b-button variant="primary" @click="submit" type="submit">Submit</b-button>
            <b-button @click="close">Close</b-button>
          </div>
        </b-form>
      </template>
    </b-modal>
    <div class="px-5">
      <h2 class="py-2">Filled Details</h2>
      <table class="table table-bordered">
        <thead class="bg-light">
          <tr>
            <th v-for="(item,index) in items" :key="index" @click="selectDob(item)">
              {{ item | capitalize }}
              <span
                :class="[(item === 'dob') ? 'arrow' : '',ascending ? 'asc'  : 'dsc']"
              ></span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row,index) in get_rows()" :key="index">
            <td>{{row.email}}</td>
            <td>{{row.name}}</td>
            <td>{{row.dob}}</td>
            <td>
              <a @click="edit(row,index)">{{row.edit}}</a>
            </td>
            <td>
              <a @click="del(row,index)">{{row.delete}}</a>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="p-3 d-flex justify-content-end">
        <div
          class="number"
          v-for="(i,index) in num_pages()"
          :key="index"
          :class="[i == currentPage ? 'active' : '']"
          @click="change_page(i)"
        >{{i}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form",
  data() {
    return {
      error: "",
      items: ["email", "name", "dob", "edit", "delete"],
      rows: [],
      form: { email: "", name: "", dob: "" },
      val: {},
      password: "",
      cpassword: "",
      status: "not_accepted",
      editIndex: false,
      indexval: "",
      ascending: true,
      currentPage: 1,
      elementsPerPage: 5
    };
  },
  methods: {
    submit() {
      if (this.form.email === "") {
        this.error = "*Email required.";
      } else if (!this.validEmail(this.form.email)) {
        this.error = "*Invalid Email.";
      } else if (this.form.name === "") {
        this.error = "*Name required.";
      } else if (this.form.dob == "") {
        this.error = "*DOB required.";
      } else if (this.password == "" || this.cpassword == "") {
        this.error = "*Password required.";
      } else if (this.password != this.cpassword) {
        this.error = "*Password not match";
      } else if (this.status != "accepted") {
        this.error = "*please accepts terms and conditions";
      } else {
        if (this.editIndex == true) {
          this.val = {
            email: this.form.email,
            name: this.form.name,
            dob: this.form.dob,
            edit: "Edit",
            delete: "Delete"
          };
          this.rows.splice(
            this.indexval + (this.currentPage - 1) * this.elementsPerPage,
            1,
            this.val
          );
          this.editIndex = false;
        } else {
          this.val = {
            email: this.form.email,
            name: this.form.name,
            dob: this.form.dob,
            edit: "Edit",
            delete: "Delete"
          };
          this.rows.push(this.val);
        }
        this.$refs["my-modal"].hide();
      }
    },
    del(row, index) {
      if (
        this.rows.length ==
        (this.currentPage - 1) * this.elementsPerPage + 1
      ) {
        this.$delete(this.rows, (this.currentPage - 1) * this.elementsPerPage);
        this.currentPage = this.currentPage - 1;
        if (this.currentPage == 0) {
          this.currentPage = 1;
        }
      } else {
        this.$delete(
          this.rows,
          index + (this.currentPage - 1) * this.elementsPerPage
        );
      }
    },
    edit(item, index) {
      this.form.email = item.email;
      this.form.name = item.name;
      this.form.dob = item.dob;
      this.editIndex = true;
      this.indexval = index;
      this.$refs["my-modal"].show();
    },
    close() {
      this.$refs["my-modal"].hide();
    },
    clear() {
      this.form = { email: "", name: "", dob: "" };
      this.password = "";
      this.cpassword = "";
      this.status = "not_accepted";
      this.error = "";
      this.editIndex = false;
    },
    validEmail(email) {
      //eslint-disable-next-line
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
    selectDob(item) {
      if (item === "dob") {
        this.sortByDob(this.rows);
      }
    },
    sortByDob(rows) {
      if (rows.length < 2) {
        this.ascending === true;
      } else {
        if (this.ascending === true) {
          rows.sort(function(a, b) {
            return new Date(b.dob) - new Date(a.dob);
          });
          this.ascending = false;
        } else {
          rows.sort(function(a, b) {
            return new Date(a.dob) - new Date(b.dob);
          });
          this.ascending = true;
        }
      }
    },
    num_pages: function() {
      return Math.ceil(this.rows.length / this.elementsPerPage);
    },
    get_rows: function() {
      var start = (this.currentPage - 1) * this.elementsPerPage;
      var end = start + this.elementsPerPage;
      return this.rows.slice(start, end);
    },
    change_page: function(i) {
      this.currentPage = i;
    }
  },
  filters: {
    capitalize: function(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>
<style scoped>
a {
  cursor: pointer;
  color: blue !important;
}
#error {
  color: red;
}
th,
tr {
  width: 200px;
}
.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}

.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid black;
}

.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid black;
}

.number {
  display: inline-block;
  padding: 4px 10px;
  color: #fff;
  border-radius: 4px;
  background: #717699;
  margin: 0px 5px;
  cursor: pointer;
}
.number:hover,
.number.active {
  background: rgb(42, 159, 189);
}
</style>