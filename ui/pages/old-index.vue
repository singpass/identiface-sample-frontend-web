<template>
    <section class="hero is-large">
        <div class="hero-body">
            <div class="container">
                <div class="columns">
                    <div class="column is-8">
                        <h1 class="title is-1">
                            Trusted identity.
                        </h1>
                        <h2 class="subtitle">
                            With Identiface, you can really prove that you are who you say you are. Securely.
                        </h2>
                        <nuxt-link :class="'button is-outlined is-dark'" to="/html_sdk">Check it out</nuxt-link>
                        <br>
                        <br>
                        <nuxt-link :class="'has-text-info'" to="/docs">
                            Read QuickStart docs
                            <b-icon
                                icon="arrow-right"
                                size="is-small">
                            </b-icon>
                        </nuxt-link>
                    </div>
                    <div class="column">
                        <img src="@/assets/img/secure_login.svg">
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    head () {
        return {
            title: 'Identiface | GovTech NDI',
        }
    },
}
</script>