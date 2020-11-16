<template>
  <div class="content">
    <h2>Add new contact</h2>
    <hr />
    <div class="margin-3"></div>
    <div class="add-form">
      <el-form
        :model="ruleForm"
        :rules="rules"
        ref="ruleForm"
        label-width="170px"
      >
        <el-form-item label="Name" prop="name">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>

        <el-form-item label="Phone" prop="phone">
          <el-input v-model="ruleForm.phone"></el-input>
        </el-form-item>

        <el-form-item label="E-mail" prop="email">
          <el-input v-model="ruleForm.email"></el-input>
        </el-form-item>

        <el-form-item label="State" prop="state">
          <el-input v-model="ruleForm.address.state"></el-input>
        </el-form-item>

        <el-form-item label="City" prop="city">
          <el-input v-model="ruleForm.address.city"></el-input>
        </el-form-item>

        <el-form-item label="street" prop="street">
          <el-input v-model="ruleForm.address.streetC"></el-input>
        </el-form-item>

        <el-form-item label="Zip" prop="zipcode">
          <el-input v-model="ruleForm.address.zipcode"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">
            Добавить
            <i class="el-icon-caret-right"></i>
            </el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {
        name: "",
        phone: "",
        email: "",
        address: {
            state: "",
            city: "",
            streetC: "",
            zipcode: "",
        }
      },
      contacts: [],
      rules: {
        name: [
          {
            required: true,
            message: "Введите никнейм",
            trigger: "blur",
          },
          {
            min: 3,
            message: "Length should be more 2",
            trigger: ["blur", "change"],
          },
        ],
        phone: [
          {
            required: true,
            message: "Please input phone",
            trigger: "blur",
          },
          {
            min: 6,
            message: "Length should be more 5",
            trigger: ["blur", "change"],
          },
        ],
        email: [
          {
            required: true,
            message: "Please input email address",
            trigger: "blur",
          },
          {
            type: "email",
            message: "Please input correct email address",
            trigger: ["blur", "change"],
          },
        ],
      },
    };
  },
  mounted() {
    if (localStorage.getItem("contacts")) {
      try {
        this.contacts = JSON.parse(localStorage.getItem("contacts"));
      } catch (e) {
        localStorage.removeItem("contacts");
      }
    }
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.contacts.data.push(this.ruleForm);
          this.newContact = "";
          this.saveContact();
          this.$message({
            message: "Contact added",
            type: "success",
          });
        } else {
          return false;
        }
      });
    },
    saveContact() {
        const parsed = JSON.stringify(this.contacts);
        localStorage.setItem("contacts", parsed);
    },
  },
};
</script>