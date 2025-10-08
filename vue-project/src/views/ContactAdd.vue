<template>
  <div class="page">
    <h4>Thêm Liên Hệ</h4>
    <!-- Truyền contact rỗng vào form để có dữ liệu hiển thị -->
    <ContactForm :contact="contact" @submit:contact="addContact" />
    <p class="text-danger mt-2">{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
  components: {
    ContactForm,
  },
  data() {
    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
        maritalStatus: "Độc thân", // mặc định là Độc thân
      },
      message: "",
    };
  },
  methods: {
    async addContact(data) {
      try {
        await ContactService.create(data);
        alert("Liên hệ được thêm thành công.");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error(error);
        this.message = "Có lỗi xảy ra khi thêm liên hệ.";
      }
    },
  },
};
</script>

<style scoped>
.page {
  max-width: 600px;
  margin: auto;
}
</style>
