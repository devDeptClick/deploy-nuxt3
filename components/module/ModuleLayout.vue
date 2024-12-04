<script setup>
import ModuleBanner from './ModuleBanner.vue';
import ModuleStatistic from './ModuleStatistic.vue';
import ModuleBenefit from './ModuleBenefit.vue';
import ModuleCustomer from './ModuleCustomer.vue';
import ModuleProduct from './ModuleProduct.vue';
import ModuleProductBestSale from './ModuleProductBestSale.vue';
import ModuleProductHot from './ModuleProductHot.vue';
import ModuleFeature from './ModuleFeature.vue';
import ModulePartner from './ModulePartner.vue';
import ModuleAboutUs from './ModuleAboutUs.vue';
import ModuleHistory from './ModuleHistory.vue';
import ModuleVision from './ModuleVision.vue';
import ModuleCoreValue from './ModuleCoreValue.vue';
import ModuleMission from './ModuleMission.vue';
import ModulePersonel from './ModulePersonel.vue';

defineProps({
    modules: Object,
})
</script>

<template>
    <div>
        <div v-for="(module, index) in modules" :key="index">
            <template v-if="module.module_subtype === 'banner'">
                <ModuleBanner :module_metadata="JSON.parse(module.module_metadata)" />
            </template>

            <template v-if="module.module_subtype === 'aboutUs'">
                <ModuleAboutUs :module="module" />
            </template>

            <template v-if="module.module_subtype === 'statistic'">
                <ModuleStatistic :module_metadata="JSON.parse(module.module_metadata)" />
            </template>

            <template v-if="module.module_subtype === 'benefits'">
                <ModuleBenefit :module="module" />
            </template>

            <template v-if="module.module_subtype === 'customer'">
                <ModuleCustomer :module="module" />
            </template>

            <template v-if="module.module_subtype === 'product'">
                <ModuleProduct :module="module" />
            </template>

            <template v-if="module.module_subtype === 'productBestSale'">
                <ModuleProductBestSale :module="module" />
            </template>

            <template v-if="module.module_subtype === 'productHot'">
                <ModuleProductHot :module="module" />
            </template>

            <template v-if="module.module_subtype === 'history'">
                <ModuleHistory :module="module" />
            </template>

            <template v-if="module.module_subtype === 'features'">
                <ModuleFeature :module="module" />
            </template>


            <template v-if="module.module_subtype === 'partner'">
                <ModulePartner :module="module" />
            </template>

            <template v-if="module.module_subtype === 'vision'">
                <ModuleVision :module="module" />
            </template>

            <template v-if="module.module_subtype === 'cores_value'">
                <ModuleCoreValue :module="module" />
            </template>

            <template v-if="module.module_subtype === 'mission'">
                <ModuleMission :module="module" />
            </template>

            <template v-if="module.module_subtype === 'personnel'">
                <ModulePersonel :module="module" />
            </template>
        </div>
    </div>
</template>

<style scoped></style>
