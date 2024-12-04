<script setup>
// import TheLayout from './components/TheLayout.vue'
import './assets/main.css'
import "bootstrap/dist/css/bootstrap.min.css"
</script>

<template>
  <div>
    <div id="ht-preloader" style="display:none">
      <div class="loader clear-loader">
        <span class="loader-div"> </span>
      </div>
    </div>

    <!-- <Suspense>
      <template #default>
        <TheLayout>
          <main>
            <NuxtPage />
          </main>
        </TheLayout>
      </template>
    </Suspense> -->

    
    <NuxtLayout :name="default">
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>

<style scoped>
#ht-preloader {
  background: #ffffff;
  bottom: 0;
  height: 100%;
  left: 0;
  overflow: hidden !important;
  position: fixed;
  right: 0;
  text-align: center;
  top: 0;
  width: 100%;
  z-index: 99999;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loader-div {
  width: 48px;
  height: 48px;
  display: inline-block;
  position: relative;
}

.loader-div::after,
.loader-div::before {
  content: '';
  width: 48px;
  height: 48px;
  border: 2px solid #000000;
  position: absolute;
  left: 0;
  top: 0;
  animation: rotationBreak 3s ease-in-out infinite alternate;
}

.loader-div::after {
  border-color: #003789;
  animation-direction: alternate-reverse;
}

@keyframes rotationBreak {
  0% {
    transform: rotate(0);
  }

  25% {
    transform: rotate(90deg);
  }

  50% {
    transform: rotate(180deg);
  }

  75% {
    transform: rotate(270deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
</style>
