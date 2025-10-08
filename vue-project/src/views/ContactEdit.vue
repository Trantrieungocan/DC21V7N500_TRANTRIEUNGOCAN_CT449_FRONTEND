<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh Liên hệ</h4>
    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
    />
    <p class="text-danger mt-2">{{ message }}</p>
  </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
  components: { ContactForm },
  props: { id: { type: String, required: true } },
  data() {
    return {
      contact: null,
      message: "",
    };
  },
  methods: {
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);

        // Khởi tạo các trường mặc định nếu backend không trả về
        if (!this.contact.maritalStatus) this.contact.maritalStatus = "Độc thân";
        if (typeof this.contact.favorite === "undefined") this.contact.favorite = false;
      } catch (error) {
        console.error(error);
        // Nếu không tìm thấy contact, chuyển hướng sang notfound
        this.$router.push({
          name: "notfound",
          params: { pathMatch: this.$route.path.split("/").slice(1) },
          query: this.$route.query,
          hash: this.$route.hash,
        });
      }
    },

    async updateContact(data) {
      try {
        await ContactService.update(this.contact._id, data);
        alert("Liên hệ được cập nhật thành công.");
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.error(error);
        this.message = "Có lỗi xảy ra khi cập nhật liên hệ.";
      }
    },

    async deleteContact() {
      if (confirm("Bạn muốn xóa Liên hệ này?")) {
        try {
          await ContactService.delete(this.contact._id);
          this.$router.push({ name: "contactbook" });
        } catch (error) {
          console.error(error);
          this.message = "Có lỗi xảy ra khi xóa liên hệ.";
        }
      }
    },
  },

  created() {
    this.getContact(this.id);
    this.message = "";
  },
};
</script>

<style scoped>
.page {
  max-width: 600px;
  margin: auto;
}
</style>
