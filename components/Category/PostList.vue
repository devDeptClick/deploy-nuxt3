<script setup>
import ThePaginate from "../Paginate/ThePaginate.vue";
import moment from "moment";
import truncate from "truncate-html";

defineProps({
    categories: Object
})

const truncateString = (str, num) => {
    if (str.length <= num) {
        return str
    }
    return str.slice(0, num) + '...'
}
</script>

<template>
    <div>
        <section>
            <div class="container">
                <div class="row" style="gap: 20px 0;">
                    <div class="col-lg-3 col-md-6" v-for="(post, index) in categories.data.data" :key="index">
                        <div class="post-card post-classic">
                            <div class="post-image">
                                <NuxtLink :to="'/tin-tuc/' + post.post_slug + '/' + post.post_id">
                                    <img class="img-fluid rounded" :src="post.post_thumbnail" :alt="post.post_title">
                                </NuxtLink>
                            </div>
                            <div class="post-desc">
                                <ul class="list-inline post-bottom mb-0">
                                    <li class="list-inline-item">
                                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"
                                            style="width: 14px;vertical-align: text-top;margin-right: 5px;">
                                            <path
                                                d="M400 64h-48V12c0-6.6-5.4-12-12-12h-40c-6.6 0-12 5.4-12 12v52H160V12c0-6.6-5.4-12-12-12h-40c-6.6 0-12 5.4-12 12v52H48C21.5 64 0 85.5 0 112v352c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48V112c0-26.5-21.5-48-48-48zm-6 400H54c-3.3 0-6-2.7-6-6V160h352v298c0 3.3-2.7 6-6 6z" />
                                        </svg>
                                        <span> {{ moment(post.created_at).format("DD-MM-YYYY") }} </span>
                                    </li>
                                    <li class="list-inline-item">
                                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512"
                                            style="width: 15px; vertical-align: baseline;margin-right: 5px;">
                                            <path
                                                d="M288 144a110.9 110.9 0 0 0 -31.2 5 55.4 55.4 0 0 1 7.2 27 56 56 0 0 1 -56 56 55.4 55.4 0 0 1 -27-7.2A111.7 111.7 0 1 0 288 144zm284.5 97.4C518.3 135.6 410.9 64 288 64S57.7 135.6 3.5 241.4a32.4 32.4 0 0 0 0 29.2C57.7 376.4 165.1 448 288 448s230.3-71.6 284.5-177.4a32.4 32.4 0 0 0 0-29.2zM288 400c-98.7 0-189.1-55-237.9-144C98.9 167 189.3 112 288 112s189.1 55 237.9 144C477.1 345 386.7 400 288 400z" />
                                        </svg>
                                        <span class="cat-links">
                                            <span>{{ post.post_view ? post.post_view : 1 }}</span>
                                        </span>
                                    </li>
                                </ul>
                                <div class="post-title">
                                    <h4>
                                        <NuxtLink :to="'/tin-tuc/' + post.post_slug + '/' + post.post_id">{{
                                            truncateString(post.post_title, 48) }}
                                        </NuxtLink>
                                    </h4>
                                </div>
                                <div v-html="truncate(post.post_brief, 72, { stripTags: true })" class="mb-3"></div>
                                <NuxtLink :to="'/tin-tuc/' + post.post_slug + '/' + post.post_id" class="primary-btn">
                                    <span>Xem thêm</span>
                                </NuxtLink>
                            </div>
                        </div>
                    </div>
                </div>

                <div>
                    <ThePaginate :links="categories.data.links.slice(1, -1)" />
                </div>
            </div>
        </section>
    </div>

</template>

<style scoped>
a {
    color: #12181c;
}

.img-fluid:hover {
    transform: scale(1.1);
}

.post-card .post-bottom li+li:before {
    display: inline-block;
    padding-right: 0.2rem;
    color: var(--themeht-primary-color);
    content: "\f7a5";
    font-family: 'Line Awesome Free';
    font-weight: 700;
}

.post-card .post-desc h4 a {
    color: #12181c;
    display: inline-block;
}

.post-title h4 {
    font-size: 18px !important;
    margin: 0;
    text-transform: capitalize;
    font-weight: 600;
    word-break: break-word;
}

.post-title {
    margin: 20px 0;
    border-top: 1px solid #e1e1e1;
    padding-top: 20px;
}

.post-classic .post-desc {
    padding-top: 30px;
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
