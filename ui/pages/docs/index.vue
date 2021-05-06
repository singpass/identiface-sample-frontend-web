<template>
    <section>
        <div class="container-fluid">
            <div class="hero" style="margin-top:2rem;">
                <div class="hero-body">
                    <div class="container">
                        <h1 class="title is-1">QuickStart Docs</h1>
                        <p class="subtitle">
                            This includes a brief reference documentation for the plug &amp; play Identiface module for demonstration purposes only. Use the code snippets and examples
                            to dive right into experimenting with the NDI Identiface biometrics SDK – <b>Face Verify</b>.
                        </p>
                        <nuxt-link :class="'has-text-info'" :to="{path: '/html_sdk'}">
                            I'm ready to go live
                            <b-icon
                                icon="arrow-right"
                                size="is-small">
                            </b-icon>
                        </nuxt-link>
                    </div>
                </div>
            </div>
        </div>
        <!-- Step 0 -->
        <client-only>
        <div class="columns">
            <div class="column is-5">
                <div class="box">
                    <div class="content">
                        <h1 class="is-title is-3">Step 0;</h1>
                        <p>
                        <strong>Use our base template.</strong>
                        <br>
                        This will get you quickly up-to-speed with the development without having to worry about basic styling.
                        <br>
                        <br>
                        In this example, you will include external CSS libraries from <a href="https://getbootstrap.com/" target="_blank">Bootstrap</a> or <a href="https://bulma.io/" target="_blank">Bulma</a>, and a simple form for the user to input
                        their NRIC/FIN numbers.
                        <br>
                        <br>
                        You'll also have to include the Identiface CDN script on your page. Link to the CDN:
                        <code>
                            {{scriptURL}}
                        </code>
                        </p>
                    </div>
                </div>
            </div>
            <div class="column is-7">
                <b-tabs v-model="activeTab">
                    <b-tab-item label="HTML + jQuery">
                        <JQuerySnippets :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                    <b-tab-item label="Svelte">
                        <SvelteSnippets :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                    <b-tab-item label="Vue.js">
                        <VueSnippets :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                </b-tabs>
            </div>
        </div>
        <hr>
        <!-- Step 1 -->
        <div class="columns">
            <div class="column is-5">
                <div class="box">
                    <div class="content">
                        <h1 class="is-title is-3">Step 1;</h1>
                        <p>
                        <strong>Retrieve user token</strong>
                        <br>
                        Call our QuickStart backend service to retrieve the user token on <code>{{baseURL + "/face/verify/token"}}</code>. You need
                        our backend secret <code>{{pw}}</code>.
                        <br>
                        <br>
                        The user token will only be returned if the NRIC
                        is valid as this will be used to verify the user's facial features.
                        <br>
                        <br>
                        Once the user token is returned, append the <code>&lt;sp-face&gt;</code> div, along with the user token into <code>&lt;div id="sp-face-here"&gt;</code>.
                        Then, append custom slots as required into the <code>&lt;div id="sp-face-slots"&gt;</code>.
                        <br>
                        </p>
                    </div>
                </div>
            </div>
            <div class="column is-7">
                <b-tabs v-model="activeTab">
                    <b-tab-item label="HTML + jQuery">
                        <JQuerySnippets1 :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                    <b-tab-item label="Svelte">
                        <SvelteSnippets1 :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                    <b-tab-item label="Vue.js">
                        <VueSnippets1 :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                </b-tabs>
            </div>
        </div>
        <hr>
        <!-- Step 2 -->
        <div class="columns">
            <div class="column is-5">
                <div class="box">
                    <div class="content">
                        <h1 class="is-title is-3">Step 2;</h1>
                        <p>
                        <strong>Validate results</strong>
                        <br>
                        Once the user has done their verification on the Identiface module, you will then need to verify
                        the validity of the results. To do this, add an event listener to the custom slots in Step 1
                        with the <code>&lt;div&gt;</code> id <code>validateResults</code>.
                        <br>
                        <br>
                        This is to prevent man-in-the-middle attacks and to ensure
                        that you have received user verification only from our Identiface backend. <code>{{baseURL + "/face/verify/validate"}}</code>
                        <br>
                        <br>
                        In this QuickStart, we have implemented collecting the client's <code>IP Address</code> and <code>User-Agent</code> for you. In your
                        own implementation, collect both details in the backend to prevent man-in-the-middle attacks.
                        </p>
                    </div>
                </div>
            </div>
            <div class="column is-7">
                <b-tabs v-model="activeTab">
                    <b-tab-item label="HTML + jQuery">
                        <JQuerySnippets2 :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                    <b-tab-item label="Svelte">
                        <SvelteSnippets2 :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                    <b-tab-item label="Vue.js">
                        <VueSnippets2 :baseURL="this.baseURL" :pw="this.pw" :singpassURL="this.singpassURL" :scriptURL="this.scriptURL" :serviceID="this.serviceID"/>
                    </b-tab-item>
                </b-tabs>
            </div>
        </div>
        <hr>
        <!-- Step 3 -->
        <div class="columns">
            <div class="column is-6 is-offset-3" style="margin-bottom: 4rem; margin-top: 4rem;">
                <div class="box">
                    <div class="content">
                        <h1 class="is-title is-3">That's all</h1>
                        <p>In this demo, we will be using a dummy NRIC persona <code>G2957839M</code>, which will return an always <code>true</code> result when verifying the identity, regardless of the face.</p>
                        <p>If you'd like to test a flow that will <b>fail</b> authentication, please replace the dummy NRIC persona to <code>G2834561K</code>. This will return a <code>false</code> match result.</p>
                        <!-- <p>Let us know any feedback on this tutorial <a>here</a>.</p> -->
                        Check out the demo <nuxt-link :class="'has-text-info'" :to="{path: '/html_sdk'}">here</nuxt-link>.
                        <br>
                    </div>
                </div>
            </div>
        </div>
        </client-only>
    </section>
</template>

<script>
import JQuerySnippets from '@/components/step_0/snippet-jquery.vue'
import JQuerySnippets1 from '@/components/step_1/snippet-jquery.vue'
import JQuerySnippets2 from '@/components/step_2/snippet-jquery.vue'

import VueSnippets from '@/components/step_0/snippet-vue.vue'
import VueSnippets1 from '@/components/step_1/snippet-vue.vue'
import VueSnippets2 from '@/components/step_2/snippet-vue.vue'

import SvelteSnippets from '@/components/step_0/snippet-svelte.vue'
import SvelteSnippets1 from '@/components/step_1/snippet-svelte.vue'
import SvelteSnippets2 from '@/components/step_2/snippet-svelte.vue'

export default {
    head () {
        return {
            title: 'Identiface | Documentation',
            script: [
                { src: 'prism.js'},
            ],
            link: [
                {rel: 'stylesheet', href: 'prism.css'}
            ],
        }
    },
    mounted() {
        if (location.hostname != "localhost") {
            this.baseURL = "https://" + location.hostname + "/api"
        }
    },
    methods: {
    },
    data() {
        return {
            activeTab: 0,
            pw: "ndi-api",
            // change this to your backend if needed
            baseURL: "http://localhost:9000",
            singpassURL: "https://stg-bio-stream.singpass.gov.sg",
            scriptURL: "https://bio-face.s3-ap-southeast-1.amazonaws.com/html5sdk/spface.js",
            serviceID: "Singpass",
        }
    },
    components: {
        VueSnippets,
        VueSnippets1,
        VueSnippets2,
        JQuerySnippets,
        JQuerySnippets1,
        JQuerySnippets2,
        SvelteSnippets,
        SvelteSnippets1,
        SvelteSnippets2,
    },
}
</script>