<template>
    <section>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
            <code class="language-javascript" ref="code">
            
            validateResult() {
                let r = axios.post(this.baseURL + "/face/verify/validate", {
                    "user_id": this.user_id,
                    "service_id": this.service_id,
                    "token": this.token,
                    "pw": this.pw
                }).then((response) => {
                    var vResponse = response.data
                    console.log(vResponse)
                    if (vResponse.type == "success") {
                        var hasPassed = vResponse.is_passed
                        var reason = vResponse.reason
                        var score = Math.ceil(vResponse.score * 100)

                        // set the minimum score you will allow to pass
                        var minThreshold = 70

                        if (hasPassed && score > minThreshold) {
                            this.alerts("Passed matching", "is-success")
                        } else {
                            this.alerts("Failed verification! Match score: " + vResponse.score, "is-danger")
                        }
                    } else {
                        var message = vResponse.message
                        this.alerts(message.error_description, "is-danger")
                    }
                }).catch((error) => {

                    if (error.response != undefined) {
                        var r = error.response.data
                        var message = r.message.error_description
                        this.alerts(message, "is-danger")
                    } else {
                        this.alerts("ERROR: Service is offline", "is-danger")
                    }
                })
            }
            </code>
        </pre>
    </section>
</template>

<script>
export default {
    props: {
        baseURL: String,
        singpassURL: String,
        pw: String,
        serviceID: String,
    },
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