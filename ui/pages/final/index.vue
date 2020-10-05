<template>
    <section class="section">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <div class="card">
                    <div class="card-content">
                        <div class="content">
                            <h3 class="subtitle">{{this.title}}<br></h3>
                            <a class="button is-dark" :href="restartURL">{{this.buttonTitle}}</a>
                        </div>
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
            title: 'Identiface | Next Actions',
        }
    },
    created() {
        if (this.$route.query.token) {
            this.token = this.$route.query.token
            this.user_id = this.$route.query.user_id
            this.service_id = this.$route.query.service_id
        } else {
            this.title = "Done!"
        }
    },
    mounted() {
        // if (this.$route.query.type == "transactions") {
        //     this.restartURL = "/?type=transactions"
        // }

        let query = Object.assign({}, this.$route.query)

        if (this.token != "") {
            this.validateResult()
            delete query.token
            delete query.user_id
            delete query.service_id
        } else {
            this.restartURL = "/sample-app/employee-image"
            this.buttonTitle = "Re-upload profile picture"
        }

        this.$router.replace({ query })

    },
    data() {
        return {
            token: "",
            user_id: "",
            service_id: "",
            title: "",
            restartURL: "/sample-app/",
            buttonTitle: "Restart verification",

        }
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
        validateResult() {
            let r = this.$axios.$post("/face/verify/validate", {
                "user_id": this.user_id,
                "service_id": this.service_id,
                "token": this.token
            }).then((vResponse) => {

                if (vResponse.type == "success") {
                    var hasPassed = vResponse.is_passed
                    var reason = vResponse.reason
                    var score = Math.ceil(vResponse.score * 100)

                    // set the minimum score you will allow to pass
                    var minThreshold = 70

                    if (hasPassed && score > minThreshold) {
                        this.alerts("Passed matching", "is-success", 5000)

                        if (this.$route.query.type == "transactions") {
                            this.title = "You have successfully transferred the funds."
                        } else {
                            this.title = "You have successfully registered your account!"
                        }
                        
                        this.buttonTitle = "Done"
                        this.restartURL = "/sample-app/"
                    } else {
                        this.alerts("Failed verification", "is-danger", 5000)
                        this.title = "Couldn't verify your identity. Retry?"
                        if (hasPassed) {
                            this.alerts("Match score: " + vResponse.score, "is-warning", 5000)
                        }
                    }
                } else {
                    var message = vResponse.message
                    console.log(vResponse)
                    this.alerts(message.error_description, "is-danger", 5000)
                    this.title = "Oh no! Something went wrong."
                }
            }).catch((error) => {

                if (error.response != undefined) {
                    var r = error.response.data
                    var message = r.message.error_description
                    this.title = message
                    this.alerts(message, "is-danger", 2000)
                } else {
                    this.title = "Verification service is offline."
                    this.alerts("ERROR: Service is offline", "is-danger", 5000)
                }
            })
        }
    },
}
</script>