<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <!-- Tên -->
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <!-- Email -->
    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <!-- Địa chỉ -->
    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <!-- Điện thoại -->
    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <!-- Radio Tình trạng hôn nhân -->
    <div class="form-group">
      <label>Tình trạng hôn nhân</label>
      <ErrorMessage name="maritalStatus" class="error-feedback" />
      <div class="marital-status-group">
        <label class="radio-item">
          <Field
            type="radio"
            name="maritalStatus"
            value="Độc thân"
            v-model="contactLocal.maritalStatus"
          /> Độc thân
        </label>
        <label class="radio-item">
          <Field
            type="radio"
            name="maritalStatus"
            value="Đã kết hôn"
            v-model="contactLocal.maritalStatus"
          /> Đã kết hôn
        </label>
      </div>
    </div>

    <!-- Checkbox Liên hệ yêu thích -->
    <div class="form-group form-check">
      <input
        name="favorite"
        type="checkbox"
        class="form-check-input"
        v-model="contactLocal.favorite"
      />
      <label for="favorite" class="form-check-label">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>

    <!-- Nút hành động -->
    <div class="form-group">
      <button class="btn btn-primary">Lưu</button>
      <button
        v-if="contactLocal._id"
        type="button"
        class="ml-2 btn btn-danger"
        @click="deleteContact"
      >
        Xóa
      </button>
      <button type="button" class="ml-2 btn btn-danger" @click="Cancel">
        Thoát
      </button>
    </div>
  </Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";

export default {
  components: { Form, Field, ErrorMessage },
  emits: ["submit:contact", "delete:contact"],
  props: { contact: { type: Object, required: true } },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải ít nhất 2 ký tự.")
        .max(50, "Tên có nhiều nhất 50 ký tự."),
      email: yup.string().email("E-mail không đúng.").max(50, "E-mail tối đa 50 ký tự."),
      address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
      phone: yup
        .string()
        .matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "Số điện thoại không hợp lệ."),
      maritalStatus: yup
        .string()
        .oneOf(["Độc thân", "Đã kết hôn"], "Tình trạng hôn nhân không hợp lệ")
        .required("Vui lòng chọn tình trạng hôn nhân"),
    });

    return {
      contactLocal: { ...this.contact, maritalStatus: this.contact.maritalStatus || "Độc thân" },
      contactFormSchema,
    };
  },
  methods: {
    submitContact() {
      this.$emit("submit:contact", this.contactLocal);
    },
    deleteContact() {
      this.$emit("delete:contact", this.contactLocal.id);
    },
    Cancel() {
      if (window.confirm("You have unsaved changes! Do you want to leave?")) {
        this.$router.push({ name: "contactbook" });
      }
    },
  },
};
</script>

<style scoped>
@import "@/assets/form.css";

.marital-status-group {
  display: flex;
  align-items: center;
  gap: 1.5rem;
}

.radio-item {
  cursor: pointer;
}

.radio-item input[type="radio"] {
  margin-right: 0.5rem;
  vertical-align: middle;
}
</style>
