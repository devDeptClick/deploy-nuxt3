<script setup>
import { ref } from 'vue';
import TheBreadcrumb from '@/components/Breadcrumb/TheBreadcrumb.vue';

const apiURL = import.meta.env.VITE_API_URL;
const data = await $fetch(`${apiURL}/lien-he`)
const customer_title = ref("Liên Hệ");
const customer_name = ref(null);
const customer_phone = ref(null);
const customer_email = ref(null);
const customer_message = ref(null);

const submitContact = async (e) => {
    e.preventDefault();
    await $fetch(`${apiURL}/lien-he`, {
        method: "POST",
        body: {
            customer_title: customer_title.value,
            customer_name: customer_name.value,
            customer_email: customer_email.value,
            customer_phone: customer_phone.value,
            customer_message: customer_message.value
        },
        async onRequest() {
            document.getElementById("ht-preloader").style.display = "flex";
        },
        async onResponse() {
            document.getElementById("ht-preloader").style.display = "none";
            alert("Gửi Yêu cầu thành công");
        }
    })
}
</script>

<template>
    <div>
        <TheBreadcrumb :data="[]" :currentName="'Liên Hệ'" />
        <section style="padding: unset !important;">
            <div class="container">
                <div class="map mb-6 container-map" v-html="data.info.map"> </div>
            </div>
        </section>

        <section>
            <div class="container">
                <div class="row" style="gap:50px 0;">
                    <div class="col-12 col-lg-6 mb-6 mb-lg-0 contact-info">
                        <div>
                            <h4>Thông tin liên hệ</h4>
                            <div class="mt-3">
                                <i class="fa-solid fa-location-dot" style="margin-right: 5px;"></i>
                                <span style="font-weight: 600;">Địa chỉ: </span>
                                <span> {{ data.info.address ? JSON.parse(data.info.address)[0] : '' }}</span>
                            </div>

                            <div class="mt-3">
                                <i class="fa-solid fa-phone" style="margin-right: 5px;"></i>
                                <span style="font-weight: 600;">Số điện thoại: </span><span>
                                    {{ data.info.phone_number }}
                                </span>
                            </div>

                            <div class="mt-3">
                                <i class="fa-solid fa-envelope" style="margin-right: 5px;"></i>
                                <span style="font-weight: 600;">Email: </span>
                                <span> {{ data.info.email_address }}</span>
                            </div>

                            <div class="mt-3 row gap-2 ml-0">
                                <i class="fa-solid fa-clock" style="margin-right: 5px;"></i>
                                <span style="font-weight: 600;">Thời gian làm việc: </span>
                                <div class="ml-2">
                                    <div v-for="(time, index) in JSON.parse(data.info.timework)" :key="index">
                                        <span>{{ time.dateweek }}: {{ time.datetime }}</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="col-12 col-lg-6 ps-lg-10">
                        <div class="dark-bg p-7" data-bg-img="images/pattern-1.png">
                            <div class="theme-title">
                                <h5 style="font-weight: 600;">Liên Hệ Với Chúng Tôi</h5>
                                <h4>Rất vui được trả lời câu hỏi của bạn</h4>
                            </div>
                            <form id="contact-form" method="post" :action="`${apiURL}/lien-he`"
                                @submit.prevent="submitContact">
                                <div class="messages"></div>

                                <div class="form-group mb-4">
                                    <input id="form_name" type="hidden" name="customer_title" value="Liên Hệ">
                                    <input id="form_name" type="text" name="customer_name" placeholder="Họ và tên"
                                        required="required" data-error="Name is required." v-model="customer_name">
                                    <div class="help-block with-errors"></div>
                                </div>
                                <div class="form-group mb-4">
                                    <input id="form_email" type="email" name="customer_email" placeholder="Email"
                                        required="required" data-error="Valid email is required."
                                        v-model="customer_email">
                                    <div class="help-block with-errors"></div>
                                </div>
                                <div class="form-group mb-4">
                                    <input id="form_phone" type="tel" name="customer_phone" placeholder="Số điện thoại"
                                        required="required" data-error="Phone is required" v-model="customer_phone">
                                    <div class="help-block with-errors"></div>
                                </div>
                                <div class="form-group">
                                    <textarea id="form_message" name="customer_message" class="h-auto"
                                        placeholder="Lời nhắn" rows="4" required="required"
                                        data-error="Please,leave us a message." v-model="customer_message"></textarea>
                                    <div class="help-block with-errors"></div>
                                </div>
                                <div class="col mt-5">
                                    <button class="primary-btn">
                                        <span>Gửi câu hỏi</span>
                                    </button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<style scoped>
.container-map:deep(iframe) {
    width: 100%;
}

h4 {
    font-size: 24px;
    font-style: normal;
    margin-bottom: 10px;
    font-weight: 600;
    line-height: 34px;
}

.dark-bg {
    background-color: #0f171b;
}

.p-7 {
    padding: 1.75rem 3rem !important;
}

.theme-title h5 {
    position: relative;
    display: inline-block;
    font-weight: 600;
    text-transform: uppercase;
    margin-bottom: 10px;
    color: #003789;
}

.theme-title h4 {
    margin-bottom: 15px;
    color: #ffffff;
}

input {
    border: none;
    border-bottom: 1px solid #e1e1e1;
    padding: .375rem 15px .375rem 0;
    width: 100%;
    height: 55px;
    color: #4a5156;
    border-radius: 0;
    background-color: transparent;
    color: #ffffff;
}

input::placeholder,
textarea::placeholder {
    color: #ffffff;
}

textarea {
    border: none;
    outline: none;
    border-bottom: 1px solid #e1e1e1;
    padding: .375rem 15px .375rem 0;
    width: 100%;
    height: 55px;
    color: #ffffff;
    background-color: transparent;
}

.dark-bg textarea:focus {
    color: #ffffff;
    border-bottom: 1px solid #003789 !important;
    box-shadow: none;
    background: transparent;
    border: none;
}

.dark-bg input:focus {
    border-bottom: 1px solid #003789 !important;
    box-shadow: none;
    background: transparent;
}

.primary-btn {
    position: relative;
    transition: all 0.6s;
}

.primary-btn,
.white-btn {
    background: none;
    padding: 16px 35px;
    overflow: hidden;
    border-radius: 0;
    position: relative;
    position: relative;
    display: inline-block;
    cursor: pointer;
    outline: none;
    border: 0;
    vertical-align: middle;
    text-decoration: none;
}

.primary-btn::before {
    left: 4px;
    z-index: 1;
    opacity: 0;
    background: #003789;
    transform: scale(0.1, 1);
}

.primary-btn::after {
    transition: all 0.6s;
    background: #003789;
}

.primary-btn::before,
.primary-btn::after {
    content: '';
    position: absolute;
    transition: all 0.6s;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
}

.primary-btn:hover::before {
    opacity: 1;
    transform: scale(1, 1);
}

.primary-btn:hover::after {
    transform: scale(1, .1);
    opacity: 0;
}

.primary-btn span {
    transition: all 0.6s;
    z-index: 9;
    position: relative;
    color: #ffffff;
}
</style>
