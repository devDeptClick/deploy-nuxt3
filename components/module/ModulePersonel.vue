<script setup>
defineProps({
    module: Object,
})
</script>

<template>
    <section class="personal-section background-css position-relative team-section">
        <div class="container">
            <div class="sec-title sec-title-center wow fadeInDown animated animated" data-wow-delay="1s"
                style="visibility: visible; animation-delay: 1s;">
                <h2 class="title-section title fw-bold">
                    <p><span style="color:#003789;">Đội Ngũ</span> Nhân Sự Hỗ Trợ</p>
                </h2>
                <div class="desc-section" v-html="module.module_description"></div>
            </div>

            <div class="row mt-3">
                <div class="col-lg-3 col-md-6"
                    v-for="(person, index) in JSON.parse(module.module_metadata).content_data" :key="index">
                    <div class="team-member">
                        <h5>
                            {{ person.item_slide_title }}
                        </h5>
                        <div class="team-images">
                            <img class="img-fluid" :src="person.item_slide_image" :alt="person.item_slide_title">
                        </div>
                    </div>

                    <div class="mt-3 mb-2 text-center fw-bold" style="color:#003789;">{{ person.item_slide_value }}
                    </div>

                    <div class="mb-3 text-center">
                        {{ person.item_slide_desc }}
                    </div>
                </div>

            </div>
        </div>
    </section>
</template>

<style scoped>
.title p {
    font-size: 44px;
    line-height: 1.5;
}

.title span {
    font-size: 44px;
    line-height: 54px;
}

.sec-title h2 {
    position: relative;
    font-size: 60px;
    font-weight: 900;
    text-transform: uppercase;
    line-height: 70px;
    letter-spacing: -1px;
}

.fw-bold {
    font-weight: 700 !important;
}

.row {
    gap: 20px 0;
}

.team-member {
    position: relative;
    overflow: hidden;
}
</style>
