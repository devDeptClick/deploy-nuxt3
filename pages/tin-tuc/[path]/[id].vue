<script setup>
import { useRoute } from 'vue-router'
import moment from "moment";
import ProductRelated from '@/components/product/ProductRelated.vue';
import PostRelated from '@/components/Post/PostRelated.vue';
import TheBreadcrumb from '@/components/Breadcrumb/TheBreadcrumb.vue';
import ModuleLayout from '@/components/module/ModuleLayout.vue';
import isMobile from 'is-mobile';

const url = import.meta.env.VITE_URL;
const apiURL = import.meta.env.VITE_API_URL;
const route = useRoute()

const data = await $fetch(`${apiURL}/${route.params.path}-p${route.params.id}?view=${!isMobile() ? 'desktop' : 'mobile'}`)

useSeoMeta({
    title: data.news.post_title,
    ogTitle: data.news.post_title,
    description: data.news.post_title,
    ogDescription: data.news.post_title,
    ogImage: data.news.post_thumbnail,
    twitterCard: 'summary_large_image',
})

useHead({
    script: [{
        type: 'application/ld+json',
        innerHTML:
        {
            "@context": "https://schema.org",
            "@type": "NewsArticle",
            "headline": data.news.post_title,
            "image": [data.news.post_thumbnail],
            "datePublished": data.news.created_at,
            "author": [{ "@type": "Person", "name": "Admin", "url": url }]
        }
    }],
})

useHead({
    script: [{
        type: 'application/ld+json',
        innerHTML:
        {
            "@context": "https://schema.org",
            "@type": "BreadcrumbList",
            "itemListElement":
                [
                    {
                        "@type": "ListItem",
                        "position": 1,
                        "item":
                        {
                            "@id": url,
                            "name": "Trang chủ"
                        }
                    },
                    {
                        "@type": "ListItem",
                        "position": 2,
                        "item":
                        {
                            "@id": url + route.fullPath,
                            "name": data.news.post_title
                        }
                    }
                ]
        }
    }],
})

</script>

<template>
    <div>
        <TheBreadcrumb :data="data.categories" :currentName="data.news.post_title" />
        <section class="single-post">
            <div class="container" style="border-bottom: 1px solid #003789;">
                <div class="row">
                    <div class="col-12">
                        <div class="post-card shadow-none">
                            <img class="img-fluid" :src="data.news.post_thumbnail" :alt="data.news.post_title"
                                :title="data.news.post_title" style="width: 100%;">
                            <div class="post-desc">
                                <div class="post-meta">
                                    <ul class="list-inline post-bottom">
                                        <li class="list-inline-item">
                                            <i class="las la-calendar-alt mr-1"></i>
                                            <span> {{ moment(data.news.created_at).format("DD-MM-YYYY") }} </span>
                                        </li>
                                        <li class="list-inline-item">
                                            <i class="las la-eye mr-1" style="vertical-align: text-bottom;"></i>
                                            <span class="cat-links">
                                                <span>106</span>
                                            </span>
                                        </li>
                                    </ul>
                                </div>

                                <div class="fw-bold" v-html="data.news.post_sapo"></div>
                                <div class="fw-bold" v-html="data.news.post_brief"></div>
                                <div v-html="data.news.post_content"></div>

                            </div>
                        </div>
                    </div>
                </div>

                <div class="social__sharing mt-3">
                    <ul class="social social-round"
                        style="display: flex; gap:5px; overflow: hidden; list-style: none; padding: 0;">
                        <li class="social-item">
                            <iframe
                                :src="`https://www.facebook.com/plugins/share_button.php?href=${url + route.fullPath}&amp;layout=button_count&amp;size=small&amp;appId=242142420396341&amp;width=119&amp;height=20`"
                                width="90" height="20" style="border:none;overflow:hidden"
                                allow="encrypted-media"></iframe>
                        </li>
                        <li class="social-item">
                            <div class="zalo-share-button" :data-href="url + $route.fullPath"
                                data-oaid="579745863508352884" data-layout="1" data-color="blue" data-customize="false"
                                style="position: relative; display: inline-block; width: 70px; height: 20px;"><iframe
                                    id="887c8dea-edf5-4600-9946-45dcd4547db9"
                                    name="887c8dea-edf5-4600-9946-45dcd4547db9" frameborder="0" allowfullscreen=""
                                    scrolling="no" width="70px" height="20px"
                                    :src="`https://button-share.zalo.me/share_inline?id=887c8dea-edf5-4600-9946-45dcd4547db9&amp;layout=1&amp;color=blue&amp;customize=false&amp;width=70&amp;height=20&amp;isDesktop=true&amp;url=${url + route.fullPath}&amp;d=eyJ1cmwiOiJodHRwczovL3Z1YW5oaG91c2Uudm4vY2FtZXJhLWlwLWltb3UtYTIteG9heS0zNjAtZG8tZ2lhbS1zYXQtYW4tdG9hbi1jaG8tZ2lhLWRpbmgtcHIyMyJ9&amp;shareType=0`"
                                    style="position: absolute; z-index: 99; top: 0px; left: 0px;"></iframe>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>

            <!-- Sản phẩm liên quan -->
            <ProductRelated :related_product="data.related_product" v-if="data.related_product" />

            <!-- Bài viết liên quan-->
            <PostRelated :related_post="data.related_post" v-if="data.related_post" />

            <ModuleLayout :modules="data?.modules" />
        </section>
    </div>

</template>

<style scoped>
a {
    color: #12181c;
}

.post-card .post-bottom li+li:before {
    display: inline-block;
    padding-right: 0.2rem;
    color: #003789;
    content: "\f7a5";
    font-family: 'Line Awesome Free';
    font-weight: 700;
}

.single-post .post-card .post-desc {
    padding-top: 30px;
}

.post-desc {
    line-height: 1.7;
}
</style>
