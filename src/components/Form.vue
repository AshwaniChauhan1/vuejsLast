<template>
  <div>
    <div class="text-left pl-5 pt-5">
      <b-button v-b-modal.modal-1>Add Details</b-button>
    </div>
    <b-modal id="modal-1" ref="my-modal" hide-footer>
      <template v-slot:modal-header>
        <h4>Fill Form</h4>
      </template>
      <template v-slot>
        <b-form v-on:submit.prevent>
          <p id="error">{{error}}</p>
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
          <div class="justify-content-between d-flex py-3">
            <b-button variant="primary" @click="submit" type="submit">Submit</b-button>
            <b-button @click="close">Close</b-button>
          </div>
        </b-form>
      </template>
    </b-modal>
    <div class="p-5">
      <h1 class="py-3">Filled Details</h1>
      <b-table hover :items="items" :fields="fields">
        <template v-slot:cell(edit)="row">
          <a @click="edit(row)">Edit</a>
        </template>
        <template v-slot:cell(delete)="row">
          <a @click="del(row)">Delete</a>
        </template>
      </b-table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Form",
  data() {
    return {
      error: "",
      fields: [
        "email",
        "name",
        { key: "dob", sortable: true },
        "edit",
        "delete"
      ],
      items: [],
      form: { email: "", name: "", dob: "" },
      val: {},
      password: "",
      cpassword: "",
      status: "not_accepted",
      editIndex: false,
      indexval: ""
    };
  },
  methods: {
    submit() {
      if (this.form.email === "") {
        this.error = "Email required.";
      } else if (!this.validEmail(this.form.email)) {
        this.error = "Invalid Email.";
      } else if (this.form.name === "") {
        this.error = "Name required.";
      } else if (this.form.dob == "") {
        this.error = "DOB required.";
      } else if (this.password == "" || this.cpassword == "") {
        this.error = "Password required.";
      } else if (this.password != this.cpassword) {
        this.error = "Password not match";
      } else if (this.status != "accepted") {
        this.error = "please accepts terms and conditions";
      } else {
        if (this.editIndex == true) {
          this.val = {
            email: this.form.email,
            name: this.form.name,
            dob: this.form.dob
          };
          this.items.splice(this.indexval, 1, this.val);
          this.editIndex = false;
          this.form.email = "";
          this.form.name = "";
          this.form.dob = "";
          this.password = "";
          this.cpassword = "";
          this.status = "not_accepted";
          this.error = "";
          this.$refs["my-modal"].hide();
        } else {
          this.val = {
            email: this.form.email,
            name: this.form.name,
            dob: this.form.dob
          };
          this.items.push(this.val);
          this.form.email = "";
          this.form.name = "";
          this.form.dob = "";
          this.password = "";
          this.cpassword = "";
          this.status = "not_accepted";
          this.error = "";
          this.$refs["my-modal"].hide();
        }
      }
    },
    del(item) {
      this.$delete(this.items, item.index);
    },
    edit(item) {
      this.form.email = item.item.email;
      this.form.name = item.item.name;
      this.form.dob = item.item.dob;
      this.editIndex = true;
      this.indexval = item.index;
      this.$refs["my-modal"].show();
    },
    close() {
      this.$refs["my-modal"].hide();
      this.form = { email: "", name: "", dob: "" };
      this.password = "";
      this.cpassword = "";
      this.status = "not_accepted";
      this.error = "";
    },
    validEmail(email) {
      //eslint-disable-next-line
      var re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
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
</style>