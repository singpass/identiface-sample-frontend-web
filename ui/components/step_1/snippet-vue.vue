<template>
    <section>
        <p>Append this after the <code>&lt;script&gt;</code> tags in Step 0:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
<code class="language-javascript" ref="code">&lt;script&gt;
    var app = new Vue({
        el: '#app',
        data: {
            singpassURL: "{{singpassURL}}",
            baseURL: "{{baseURL}}",
            user_id: "",
            service_id: "{{serviceID}}",
            pw: "{{pw}}",
            token: "",
            
            message: "Please fill in your NRIC below.",
            message_colour: "message is-primary",

            form_hidden: false,
        },
        created: function() {
            window.SpfaceEvent = event => {
                switch (event.type) {
                    case "passed":
                    case "failed":
                        this.validateResult();
                        break;
                    default:
                        break;
                }
            }
        },
        methods: {
            alerts(message, colour) {
                this.message = message
                this.message_colour = "message " + colour
            },
            startVerify(e) {
                e.preventDefault()
                this.validateNRIC().then((token) => {
                    if (token != "") {
                        this.form_hidden = true
                    }
                }).catch((error) => {
                    if (error.response != undefined) {
                        var r = error.response.data
                        this.alerts(r.message, "is-danger")
                    } else {
                        this.alerts("ERROR: Service is offline", "is-danger")
                    }
                })
            },
            validateNRIC() {
                return new Promise((resolve, reject) => {
                    this.token = ""
                    let r = axios.post(this.baseURL + "/face/verify/token", {
                        "user_id": this.user_id, 
                        "service_id": this.service_id, 
                        "transaction_type": this.transaction_type, 
                        "pw": this.pw
                    }).then((response) => {
                        var tokenResponse = response.data
                        if (tokenResponse.type == "error") {
                            this.alerts(tokenResponse.status + " " + tokenResponse.message.error_description, "is-danger")
                        } else {
                            var token = tokenResponse.token
                            this.token = tokenResponse.token
                            this.nextURL += this.token + "&user_id=" + this.user_id + "&service_id=" + this.service_id
                            this.alerts("NRIC validated!", "is-info")

                            resolve(token)
                        }
                    }).catch((error) => {
                        this.token = ""
                        if (error.response != undefined) {
                            var r = error.response.data
                            this.alerts(r, "is-danger")
                        } else {
                            this.notice = "Verification service is offline."
                            this.alerts("ERROR: Service is offline", "is-danger")
                        }
                    })
                })
            },

            // Place Step 2 here

        },
    });
&lt;/script&gt;</code>
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