<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh Liên hệ</h4>

    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
    />

    <!-- Alert thông báo cập nhật thành công tùy chỉnh -->
    <div v-if="showSuccessAlert" class="alert alert-success mt-3" role="alert" id="edit-success-alert">
      Liên hệ được cập nhật thành công.
    </div>

    <!-- Alert xác nhận xóa liên hệ tùy chỉnh -->
    <div v-if="showDeleteConfirm" class="alert alert-danger mt-3" role="alert" id="edit-delete-confirm">
      <h5><i class="fas fa-exclamation-triangle"></i> Xác nhận xóa</h5>
      <p>Bạn muốn xóa Liên hệ này?</p>
      <hr>
      <div class="d-flex justify-content-end">
        <button class="btn btn-secondary btn-sm mr-2" @click="showDeleteConfirm = false">Hủy bỏ</button>
        <button class="btn btn-danger btn-sm" id="btn-confirm-delete-one" @click="confirmDeleteContact">Đồng ý xóa</button>
      </div>
    </div>

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

  props: {
    id: {
      type: String,
      required: true,
    },
  },

  data() {
    return {
      contact: null,
      message: "",
      showSuccessAlert: false,
      showDeleteConfirm: false,
    };
  },

  methods: {
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (error) {
        console.log(error);

        this.$router.push({
          name: "notfound",
          params: {
            pathMatch: this.$route.path
              .split("/")
              .slice(1),
          },
          query: this.$route.query,
          hash: this.$route.hash,
        });
      }
    },

    async updateContact(data) {
      try {
        await ContactService.update(this.contact._id, data);

        this.showSuccessAlert = true;
        setTimeout(() => {
          this.showSuccessAlert = false;
          this.$router.push({ name: "contactbook" });
        }, 2000);
      } catch (error) {
        console.log(error);
      }
    },

    deleteContact() {
      this.showDeleteConfirm = true;
    },

    async confirmDeleteContact() {
      this.showDeleteConfirm = false;
      try {
        await ContactService.delete(this.contact._id);
        this.$router.push({ name: "contactbook" });
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    this.getContact(this.id);
    this.message = "";
  },
};
</script>