<script setup>
import { ref } from 'vue';
defineProps({
    data: Object,
    dataMenu: Object,
})

const countCart = ref(0);
onMounted(() => {
    countCart.value = localStorage.getItem('cart') ? JSON.parse(localStorage.getItem('cart')).length : 0
})

const handleShowMenu = () => {
    document.getElementById('navbarNav').classList.toggle('show');
}

const handleShowMenuChild = (index) => {
    document.querySelector(`.dropdown-menu-child-${index}`).classList.toggle('show');
}

const handleShowMenuGrand = (index) => {
    document.querySelector(`.dropdown-menu-grand-${index}`).classList.toggle('show');
}

const handleShowSearch = () => {
    document.getElementById('search-input-box').style.display = 'block';
}

const handleCloseSearch = ()=>{
    document.getElementById('search-input-box').style.display = 'none';
}

</script>

<template>
    <header id="site-header" class="header">
        <div id="header-wrap">
            <div class="container">
                <div class="row position-relative">
                    <!--menu start-->
                    <div class="col">
                        <nav class="navbar navbar-expand-lg navbar-light bg-light">
                            <div class="row align-items-center">
                                <button class="navbar-toggler mx-0 mr-2 navbar-toggler-button" type="button"
                                    data-toggle="collapse" @click="handleShowMenu" data-target="#navbarNav"
                                    aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                                    <span class="navbar-toggler-icon"></span>
                                </button>

                                <NuxtLink to="/" class="navbar-brand logo">
                                    <img class="img-fluid" :src="data?.logo_footer" width="60" :alt="data.seo_title">
                                </NuxtLink>
                            </div>

                            <div class="collapse navbar-collapse" id="navbarNav">
                                <ul class="navbar-nav mx-auto">
                                    <li class="nav-item dropdown"
                                        v-for="(menu, index) in dataMenu?.menu_header ? JSON.parse(dataMenu.menu_header.menu_data) : []"
                                        :key="index">

                                        <NuxtLink class="nav-link dropdown-toggle"
                                            :class="menu.children.length > 0 ? 'dropdown-icon' : ''" :to="menu.url">
                                            {{ menu.title }}
                                        </NuxtLink>

                                        <span class="dropdown-toggle dropdown-toggle-icon" data-toggle="dropdown"
                                            @click="handleShowMenuChild(index)" v-if="menu.children.length > 0"
                                            style="position: absolute;right:-10px;top:5px">
                                        </span>

                                        <ul class="dropdown-menu" :class="`dropdown-menu-child-${index}`"
                                            v-if="menu.children.length > 0">
                                            <li class="dropdown-submenu" v-for="(child, index) in menu.children"
                                                :key="index">
                                                <NuxtLink class="dropdown-item" :to="child.url">
                                                    {{ child.title }}</NuxtLink>

                                                <span class="dropdown-toggle dropdown-toggle-icon"
                                                    v-if="child.children.length > 0" data-toggle="dropdown"
                                                    style="position: absolute;right:-10px;top:5px"
                                                    @click="handleShowMenuGrand(index)">
                                                </span>

                                                <ul class="dropdown-menu" :class="`dropdown-menu-grand-${index}`"
                                                    v-if="child.children.length > 0">
                                                    <li v-for="(grandchild, index) in child.children" :key="index">
                                                        <NuxtLink class="dropdown-item" :to="grandchild.url">
                                                            {{ grandchild.title }}</NuxtLink>
                                                    </li>
                                                </ul>
                                            </li>
                                        </ul>
                                    </li>
                                </ul>

                            </div>
                            <div class="right-nav align-items-center d-flex justify-content-end">
                                <div class="search-icon" @click="handleShowSearch">
                                    <span id="search">
                                        <i class="las la-search"></i>
                                    </span>
                                </div>

                                <div class="cart ms-4">
                                    <NuxtLink to="/gio-hang" aria-label="gio hang">
                                        <span class="white-bg px-2 py-1 shadow-sm" id="header-cart-btn"
                                            :data-cart-items="countCart">
                                            <i class="las la-shopping-cart"></i>
                                        </span>
                                    </NuxtLink>
                                </div>
                            </div>
                        </nav>
                    </div>

                    <div class="CartNotification__Wrapper-sc-1egoil-0 iiyvNb position-absolute cart-alert"
                        style="display:none">
                        <p class="status">
                            <svg stroke="currentColor" fill="currentColor" stroke-width="0" viewBox="0 0 512 512"
                                height="1em" width="1em" xmlns="http://www.w3.org/2000/svg">
                                <path
                                    d="M504 256c0 136.967-111.033 248-248 248S8 392.967 8 256 119.033 8 256 8s248 111.033 248 248zM227.314 387.314l184-184c6.248-6.248 6.248-16.379 0-22.627l-22.627-22.627c-6.248-6.249-16.379-6.249-22.628 0L216 308.118l-70.059-70.059c-6.248-6.248-16.379-6.248-22.628 0l-22.627 22.627c-6.248 6.248-6.248 16.379 0 22.627l104 104c6.249 6.249 16.379 6.249 22.628.001z">
                                </path>
                            </svg>
                            Thêm vào giỏ hàng thành công!
                        </p>

                        <NuxtLink to="/gio-hang" class="btn-view-cart">
                            Xem giỏ hàng và thanh toán
                        </NuxtLink>
                    </div>
                    <!--menu end-->
                </div>
            </div>
        </div>

        <div class="search-input" id="search-input-box">
            <div class="search-inner-box">
                <div class="container">
                    <div class="row justify-content-center">
                        <div class="col-lg-6">
                            <form role="search" id="search-form" action="/tim-kiem"
                                class="search-form d-flex justify-content-between search-inner">
                                <label>
                                    <input type="search" class="search-field" placeholder="Tm kiếm" value="" name="q">
                                </label>
                                <button type="submit" class="search-submit">
                                    <i class="las la-search"></i>
                                </button>
                                <span class="las la-times close-search" id="close-search" title="Close Search" @click="handleCloseSearch"></span>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>

</template>

<style scoped>
header {
    box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
}

#header-wrap {
    background: #f8f9fa !important;
}

.navbar-light .navbar-nav .nav-link {
    color: #0f171b;
}

.navbar-nav .nav-item.dropdown .dropdown-menu {
    background: #003789;
    box-shadow: rgb(0 0 0 / 20%) 0px 7px 29px 0px;
    border-radius: 0;
    padding: 30px;
    border: none;
}

.navbar-nav .dropdown-menu span,
.navbar-nav .dropdown-menu a {
    background: none;
    padding: 5px 0;
    padding-right: 15px;
    color: #ffffff !important;
    position: relative;
    display: inline-block;
}

.nav-item.dropdown .dropdown-menu a:before {
    opacity: 1;
    left: -17px;
    color: #ffffff;
    -webkit-animation: spin 2s linear infinite;
    animation: spin 2s linear infinite;
}

.navbar-nav .dropdown-menu a:hover {
    padding-left: 20px;
    background: none;
    color: #1176d3;
}

.nav-item.dropdown .dropdown-menu a:hover:before {
    opacity: 1;
    left: 0;
    color: #ffffff;
    -webkit-animation: spin 2s linear infinite;
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

.cart a i {
    font-size: 26px;
    color: #12181c;
}

[data-cart-items]::before {
    content: attr(data-cart-items);
    position: absolute;
    top: -.5rem;
    right: -.75rem;
    display: -webkit-box;
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    -webkit-box-pack: center;
    justify-content: center;
    width: 20px;
    height: 20px;
    font-size: 11px;
    font-weight: 500;
    border-radius: 50%;
    background-color: #1176d3;
    color: #ffffff;
}

.iiyvNb {
    position: absolute;
    bottom: 15px;
    right: 30px;
    padding: 16px;
    transform: translateY(100%);
    background-color: rgb(255, 255, 255);
    border-radius: 6px;
    box-shadow: rgb(179, 179, 179) 1px 1px 15px;
}

.iiyvNb::before {
    content: "";
    position: absolute;
    bottom: 100%;
    right: 15px;
    border-width: 8px;
    border-style: solid;
    border-color: transparent transparent rgb(255, 255, 255);
    border-image: initial;
}

.iiyvNb .status svg {
    margin-right: 4px;
    color: rgb(76, 175, 80);
    font-size: 19px;
}

.iiyvNb .status {
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    margin: 0px;
    color: rgb(51, 51, 51);
    font-size: 13px;
    white-space: nowrap;
}

.iiyvNb .btn-view-cart {
    display: block;
    margin-top: 16px;
    padding: 10px 0px;
    width: 240px;
    color: rgb(255, 255, 255);
    font-size: 14px;
    font-weight: 400;
    text-align: center;
    white-space: nowrap;
    background-color: rgb(255, 57, 69);
    border-radius: 4px;
}

@media (max-width: 991.98px) {
    .navbar-toggler {
        padding: 0;
    }

    .navbar-collapse {
        background: #ffffff;
        max-height: 400px;
        left: 0;
        padding: 20px;
        position: absolute;
        z-index: 99;
        top: 100%;
        width: 100%;
        overflow: auto;
        border: medium none;
        -webkit-box-shadow: 0 0 45px rgb(5 28 141 / 10%);
        -moz-box-shadow: 0 0 45px rgb(5 28 141 / 10%);
        box-shadow: 0 0 45px rgb(5 28 141 / 10%);
    }

    .dropdown-toggle-icon {
        top: 15px !important;
    }
}

@media (max-width: 767px) {
    #search-form {
        margin: 0 !important;
        width: 82%;
    }
}
</style>
