<template>
  <div class="contact-card">
    <div class="p-1"><strong>Tên:</strong> {{ contact.name }}</div>
    <div class="p-1"><strong>E-mail:</strong> {{ contact.email }}</div>
    <div class="p-1"><strong>Địa chỉ:</strong> {{ contact.address }}</div>
    <div class="p-1"><strong>Điện thoại:</strong> {{ contact.phone }}</div>
    <div class="p-1">
      <strong>Liên hệ yêu thích:&nbsp;</strong>
      <i v-if="contact.favorite" class="fas fa-check text-success"></i>
      <i v-else class="fas fa-times text-danger"></i>
    </div>
    <div class="p-1">
      <strong>Tình trạng hôn nhân:&nbsp;</strong>
      {{ contact.maritalStatus || "Độc thân" }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    contact: { type: Object, required: true },
  },
};
</script>

<style scoped>
.contact-card {
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 1rem;
  margin-bottom: 1rem;
  background-color: #f9f9f9;
}
.p-1 {
  margin-bottom: 0.5rem;
}
</style>
