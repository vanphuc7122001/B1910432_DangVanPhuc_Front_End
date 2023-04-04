<template>
    <div v-if="contact" class="page">
        <h4 v-if="contact._id">Hiệu chỉnh Liên hệ</h4>
        <h4 v-if="!contact._id">Thêm Liên hệ</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="updateContact"
            @delete:contact="deleteContact"
            @add:contact="addContact"
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
    props: {
        id: { type: String, required: true },
    },
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
            } catch (error) {
                console.log(error);
                // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
                this.$router.push({
                    name: "notfound",
                    params: {
                        pathMatch: this.$route.path.split("/").slice(1)
                    },
                    query: this.$route.query,
                    hash: this.$route.hash,
                });
            }
        },

        async updateContact(data) {
            try {
                await ContactService.update(this.contact._id, data);
                this.message = "Liên hệ được cập nhật thành công.";
            } catch (error) {
                console.log(error);
            }
        },

        async deleteContact() {
            if (confirm("Bạn muốn xóa Liên hệ này?")) {
                try {
                    await ContactService.delete(this.contact._id);
                    this.$router.push({ name: "contactbook" });
                } catch (error) {
                    console.log(error);
                }
            }
        },

        async addContact(data) {
            try {
                if (data.name ===  '' || data.email ===  '' || data.address ===  '' || data.phone ===  '' || data.favorite ===  false) {
                    alert('Bạn phải nhập hết tất cả các trường');
                }
                await ContactService.create(data);
                this.message = "Thêm mới liên hệ thành công.";
            } catch (error) {
                console.log(error);
            }
        },
    },
    
    created() {
        if (this.id) {
            this.getContact(this.id);
            this.message = "";
        } else if (!this.id) {
            this.contact = {
                name : '',
                email : '',
                phone : '',
                address : '',
                favorite : false

            }
        }
    },
};
</script>