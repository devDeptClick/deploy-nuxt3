<script setup>
import { ref } from 'vue';
defineProps({
    logo: String,
    logoSocial: Object,
})
const apiURL = import.meta.env.VITE_API_URL;
const data = await $fetch(`${apiURL}/footer`)
</script>

<template>
    <footer class="dark-bg footer mt-5">
        <div class="container">
            <div class="row">
                <div class="col-lg-3 col-md-6">
                    <h5 class="mb-4">Về Chúng Tôi</h5>
                    <div class="footer-menu"
                        v-html="data.footer_data ? JSON.parse(data.footer_data).column_1.content_col_1 : ''">
                    </div>
                </div>
                <div class="col-lg-4 col-md-6 mt-5 mt-lg-0">
                    <h5 class="mb-4">{{ data.footer_data ? JSON.parse(data.footer_data).column_2.title_col_2 : 'Liên Hệ'
                        }}</h5>
                    <div class="footer-cntct">
                        <ul class="media-icon list-unstyled">
                            <li>
                                <i class="flaticon-paper-plane"></i>
                                <p class="mb-0">{{ data.footer_data ?
                                    JSON.parse(data.footer_data).column_2.address_col_2 : '' }}</p>
                            </li>
                            <li>
                                <i class="flaticon-message"></i>
                                <a :href="'mailto:' + JSON.parse(data.footer_data).column_2.email_address_col_2">
                                    {{
                                        data.footer_data ? JSON.parse(data.footer_data).column_2.email_address_col_2 : ''
                                    }}
                                </a>
                            </li>
                            <li>
                                <i class="flaticon-phone-call"></i>
                                <a :href="'tel:' + JSON.parse(data.footer_data).column_2.hotline_col_2">
                                    {{ data.footer_data ? JSON.parse(data.footer_data).column_2.hotline_col_2 : '' }}
                                </a>

                            </li>
                            <li>
                                <i class="flaticon-alarm-clock"></i>
                                <p class="mb-0">{{ data.footer_data ?
                                    JSON.parse(data.footer_data).column_2.time_work_col_2 : '' }}</p>
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="col-lg-2 col-md-6">
                    <h5 class="mb-4">
                        {{ data.footer_data ? JSON.parse(data.footer_data).column_3.title_col_3 : 'Chính Sách' }}
                    </h5>

                    <div class="footer-menu"
                        v-html="data.footer_data ? JSON.parse(data.footer_data).column_3.content_col_3 : ''"></div>

                </div>
                <div class="col-lg-3 col-md-6 mt-5 mt-lg-0">
                    <h5 class="mb-4">
                        {{ data.footer_data ? JSON.parse(data.footer_data).column_4.title_col_4 : '' }}
                    </h5>

                    <div class="subscribe-form"
                        v-html="data.footer_data ? JSON.parse(data.footer_data).column_4.content_col_4 : ''"></div>

                </div>
            </div>
        </div>
        <div class="secondary-footer">
            <div class="container">
                <div class="row align-items-center">
                    <div class="col-lg-2 col-md-4">
                        <a class="footer-logo d-inline-block logo" href="index.html">
                            <img class="img-fluid" :src="logo" alt="Logo Img" width="60">
                        </a>
                    </div>
                    <div class="col-lg-4 col-md-8 mt-4 mt-md-0">

                        <ul class="list-inline p-0 m-0 social-icons footer-social">
                            <li class="list-inline-item" v-for="(social, index) in JSON.parse(logoSocial.value)"
                                :key="index">
                                <a :href="social.item_slide_url" target="_blank" aria-label="social" v-html="social.item_slide_svg" v-if="social.imgorsvg==='svg'"></a>
                                <a :href="social.item_slide_url" target="_blank" aria-label="social" v-else>
                                    <img :src="social.item_slide_image" :alt="social.item_slide_image_alt" width="24" height="24">
                                </a>
                            </li>
                        </ul>
                    </div>
                    <div class="col-lg-6 col-md-12 text-lg-end mt-4 mt-lg-0 text-white"
                        v-html="data?.footer_data ? JSON.parse(data?.footer_data).copyright : ''"></div>
                </div>
            </div>
        </div>
    </footer>
    
</template>

<style scoped>
.footer {
    padding-top: 100px;
}

.dark-bg {
    background-color: #0f171b;
}

.footer h5 {
    padding-left: 30px;
    font-weight: 500;
    position: relative;
    color: #ffffff;
}

.footer h5:before {
    content: "\f1ce";
    font-family: 'Line Awesome Free';
    font-weight: 700;
    color: #1176d3;
    font-size: 24px;
    position: absolute;
    left: 0;
    top: 0;
    animation: spin 2s linear infinite;
}

@-webkit-keyframes spin {
    0% {
        -webkit-transform: rotate(0deg);
    }

    100% {
        -webkit-transform: rotate(360deg);
    }
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}

.footer-menu {
    color: #ffffff;
}

.footer-menu:deep(p) {
    line-height: 1.7;
    margin: 0 0 15px !important;
}

.footer-menu li a {
    color: #ffffff;
    position: relative;
}

.footer-menu:deep(a) {
    color: #ffffff !important;
    position: relative;
}

.media-icon li a,
.media-icon li p {
    color: #ffffff;
}

.media-icon li i {
    font-size: 24px;
    color: #1176d3;
    vertical-align: middle;
    line-height: 24px;
    margin-right: 10px;
}

.list-inline-item:not(:last-child) {
    margin-right: .5rem;
}

.social-icons li {
    display: inline-block;
    list-style: none;
    padding: 0;
    margin: 0 8px 0 0;
}

.social-icons.footer-social li a {
    padding: 5px;
    height: 40px;
    width: 40px;
    line-height: 40px;
    font-size: 24px;
    color: #ffffff;
    background: #303b40;
}

.social-icons li:hover a {
    transform: rotateY(360deg);
}

.footer-social:deep(svg){
    width: 24px;
    height: 24px;
}
</style>
