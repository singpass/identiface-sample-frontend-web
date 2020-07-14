<template>
    <section>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
            <code class="language-javascript" ref="code">
    
            </code>
        </pre>
    </section>
</template>

<script>
export default {
    methods: {
        // Popup alerts
        alerts(message, type, duration) {
            this.$buefy.toast.open({
                duration: duration,
                message: message,
                type: type,
                queue: false,
                canvasWidth: 800,
                canvasHeight: 800,
            })
        },
        copy() {
            this.alerts("Copied to clipboard!", "is-info", 3000)
            navigator.clipboard.writeText(this.$refs.code.textContent)
        },
    }
}
</script>