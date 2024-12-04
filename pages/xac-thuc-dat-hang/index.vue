<script setup>
import { ref } from 'vue';

const route = useRoute()
const orderId = ref(route.query.order_id);
</script>

<template>
    <section>
        <div class="container">

            <div class="row">
                <div class="col-12 ">
                    <div class="message-box">
                        <div class="success-container">
                            <br>
                            <div style="padding-left: 5%; padding-right: 5%">
                                <hr>
                            </div>
                            <br>
                            <h1 class="monserrat-font" style="color: Grey">Cảm ơn bạn đã đặt hàng</h1>
                            <br>

                            <div class="confirm-green-box">
                                <br>
                                <h5>Thông tin đơn hàng</h5>
                                <p>Đơn hàng số #{{ orderId }} của bạn đã được đặt thành công!</p>
                                <p>Cảm ơn bạn đã lựa chọn chúng tôi. Bạn sẽ sớm nhận được Email xác nhận.</p>
                            </div>

                            <br>
                            <button id="create-btn" class="btn btn-ouioui-secondary margin-left-5px">
                                <NuxtLink to="/">
                                    Tiếp tục mua sắm
                                </NuxtLink>
                            </button>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </section>
    <!-- end of main -->
</template>

<style scoped>
.message-box {
    display: flex;
    justify-content: center;
    padding-bottom: 20vh;
}

.success-container {
    background: white;
    height: 480px;
    width: 100%;
    box-shadow: 5px 5px 10px grey;
    text-align: center;
}

.confirm-green-box {
    width: 100%;
    height: 160px;
    background: #d7f5da;
}


.monserrat-font {
    font-family: 'Montserrat', sans-serif;
    letter-spacing: 2px;
}
</style>
