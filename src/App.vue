<script>
import AppNav from './components/AppNav.vue';
import AppBanner from './components/AppBanner.vue';
import AppMain from './components/AppMain.vue';

export default {
    components: {
        AppNav,
        AppBanner,
        AppMain,
    },

}
</script>

<template>
    <AppNav />

    <AppBanner />

    <AppMain />
</template>

<style lang="scss">
@use './styles/general.scss';

//
</style>
