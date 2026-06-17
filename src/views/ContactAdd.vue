<template>
  <div class="page">
    <h4>Thêm Liên hệ mới</h4>
    <ContactForm
      :contact="newContact"
      @submit:contact="createContact"
    />
    <p>{{ message }}</p>
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
      newContact: {
        name: "",
        email: "",
        address: "",
        phone: "",
        favorite: false,
      },
      message: "",
    };
  },
  methods: {
    async createContact(data) {
      try {
        await ContactService.create(data);
        this.message = "Liên hệ được thêm mới thành công!";
        alert(this.message);
        this.$router.push({ name: "contactBook" });
      } catch (error) {
        console.log(error);
        this.message = "Có lỗi xảy ra khi thêm liên hệ.";
      }
    },
  },
};
</script>

<style scoped>
.page {
  max-width: 400px;
  margin: auto;
}
</style>
