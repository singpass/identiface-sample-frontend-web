<template>
    <div class="container-fluid">
        <navbar/>
        <div class="container" style="min-height:95vh;">
            <nuxt />
        </div>
    </div>
</template>

<script>
import navbar from '@/layouts/nav.vue'
export default {
    head () {
        return {
            script: [
                { src: 'https://cdn.jsdelivr.net/gh/google/code-prettify@master/loader/run_prettify.js' },
            ],
        }
    },
    components: {
        navbar,
    },
}
</script>

<style>
    .card, .box {
        background-color: #fff;
        box-shadow: 0 .5em 1em -.125em rgba(10,10,10,.1),0 0 0 1px rgba(10,10,10,.02);
        color: #4a4a4a;
    }
    .no-resize {
        resize: none !important;
        min-height: 40vh !important;
    }

    pre {
        height: 50vh !important;
    }

    .tag {
        background-color: inherit !important;
        padding-left: inherit !important;
        padding-right: inherit !important;
    }

    .tag:not(body).is-pill {
        background-color: #6943d0 !important;
        color: rgba(255, 255, 255) !important;
    }

    .token.attr-name {
        padding-left: 0.5rem !important;
    }
</style>
