<template>
    <section class="section">
        <div class="columns">
            <div class="column is-half is-offset-one-quarter has-text-centered">
                <p>{{this.notice}}</p>
            </div>
        </div>
        <div class="columns">
            <div :class="columnA">
                <b-steps
                    v-model="activeStep"
                    :animated="true"
                    :rounded="false"
                    :has-navigation="false">
                    <b-step-item step="1" label="Account" :clickable="false">
                        <h1 class="title has-text-centered">Account</h1>
                        <div class="card">
                            <div class="card-content">
                                <div class="content">
                                    <!-- For keying in of NRIC -->
                                    <form v-on:submit.prevent="startVerify">
                                        <b-field label="First name">
                                            <b-input value="John" placeholder="" required :disabled="true"></b-input>
                                        </b-field>
                                        <b-field label="Last name">
                                            <b-input value="Doe" placeholder="" required :disabled="true"></b-input>
                                        </b-field>
                                        <b-field label="NRIC/FIN number">
                                            <b-input v-model="user_id" placeholder="S7012345A" required :disabled="validated"></b-input>
                                        </b-field>
                                        <a class="is-primary is-size-7" @click="user_id = 'G2834561K'" v-if="this.user_id == 'G2957839M'"><u>Set to a dummy NRIC persona that will fail verification</u></a>
                                        <a class="is-primary is-size-7" @click="user_id = 'G2957839M'" v-else><u>Set to a dummy NRIC persona that will pass verification</u></a>
                                        <br><br>
                                        <b-button type="submit" class="is-primary" :disabled="!(!validated && this.user_id.length == 9)" v-on:click="startVerify">Next</b-button>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </b-step-item>

                    <b-step-item step="2" label="Verification" :clickable="false">
                        <h1 class="title has-text-centered">Verification</h1>
                        
                        <div class="card">
                            <div class="card-content">
                                <div class="content" ref="verifyCard">
                                    <h4>Verify your identity with SingPass Face</h4>
                                    <img src="@/assets/img/SingPassLogo.svg">
                                    <br>
                                    <a class="is-primary is-size-7"
                                        @click="isPrivacyAssuranceActive = true">
                                        Read SingPass Face Privacy Assurance
                                    </a>
                                    <br>
                                    <br>
                                    <p>Verification progress:</p>
                                    <progress id="progress-bar" class="progress is-large is-primary" value="0" max="100" hidden>0</progress>
                                    <hr>
                                    <!-- SPFace Component -->
                                    <b-loading :is-full-page="false" :active.sync="isLoading" :can-cancel="true"></b-loading>
                                    <sp-face :token="token" v-if="validated" 
                                    base_url="https://stg-bio-stream.singpass.gov.sg"
                                    logo="https://www.ndi-api.gov.sg/assets/img/ndi-api-logo.png"
                                    >
                                        <!-- Custom slots go in here -->
                                        <div id="sp-face-slots">
                                            <!-- No Camera -->
                                            <div slot="no_camera" class="m-1">
                                                <p class="has-text-danger">You don't have a camera on your device.<br></p>
                                            </div>
                                            <!-- Unsupported browser -->
                                            <!-- Permissions check -->
                                            <div slot="grant_permission" class="m-1">
                                                <p class="has-text-grey-dark">Before we proceed, please allow us to access your camera.<br></p>
                                            </div>
                                            <div slot="grant_button" class="m-1">
                                                <b-button class="is-warning">Allow camera access</b-button><br><br>
                                                <!-- Let the user verify their identity in another method -->
                                                <a class="has-text-grey" href="/"><u>Use another way to verify my identity</u></a>
                                            </div>
                                            <!-- Permissions denied -->
                                            <div slot="permission_denied" class="m-1">
                                                <p class="has-text-danger">You've denied permissions. If you would like to continue with the verification, please refresh the page.<br></p>
                                            </div>
                                            <!-- UI has access to camera and is ready to begin verification -->
                                            <div slot="ready" class="m-1">
                                                <p class="has-text-grey-dark">Thank you. Please proceed to verify your identity.<br></p>
                                            </div>
                                            <div slot="button" class="m-1">
                                                <b-button class="is-primary">Scan my face</b-button>
                                            </div>
                                            <!-- User aborted the verification process -->
                                            <div slot="aborted" class="m-1">
                                                <p class="has-text-grey-dark">If you'd like to try again, please click the button below.<br></p>
                                            </div>
                                            <!-- Verification is sent to the server and is processing -->
                                            <div slot="progress" class="m-1">
                                                <p class="has-text-grey-dark">Verifying, please wait...<br></p>
                                            </div>
                                            <!-- Passed verification -->
                                            <div slot="passed" class="m-1">
                                                <!-- Redirect to the next action on successful validation -->
                                                <a class="button is-primary" :href="nextURL">Continue</a>
                                            </div>
                                            <!-- Failed verification -->
                                            <div slot="failed" class="m-1">
                                                <!-- Redirect to the next action on successful validation -->
                                                <a class="button is-primary" :href="nextURL">Continue</a>
                                            </div>
                                            <!-- Errors -->
                                            <div slot="error" class="m-1">
                                                <p class="has-text-grey-dark">Oh no, something went wrong. Please try again.<br></p>
                                                <a class="button is-warning" href="/sample-app/registrations">Retry</a>
                                            </div>
                                        </div>
                                    </sp-face>
                                </div>
                            </div>
                        </div>
                    </b-step-item>
                </b-steps>
            </div>
        </div>
        <b-modal :active.sync="isCardModalActive" :width="640" scroll="keep">
            <div class="card">
                <div class="card-content">
                    <div class="content">
                        <p><b>Welcome to the Face Verify API demo.</b></p>
                        <p class="is-italic">The Face Verify Service contains a face-scanning module with Presentation-Attack-Detection (PAD) and liveness detection technologies that you can use to embed into your frontend, with API endpoints to validate the user's identity and results.</p>
                        <p>In this sample registration page demo, we will be using a dummy NRIC persona <code>G2957839M</code>, which will return an always <code>true</code> result when verifying the identity, regardless of the face.</p>
                        <p>If you'd like to test a flow that will <b>fail</b> authentication, please replace the dummy NRIC persona to <code>G2834561K</code>. This will return a <code>false</code> match result.</p>
                        <br>
                        <a class="button is-primary" @click="isCardModalActive = false">Close</a>
                    </div>
                </div>
            </div>
        </b-modal>
        <client-only>
        <b-modal :active.sync="isPrivacyAssuranceActive" :width="640" scroll="keep">
            <div class="card">
                <div class="card-content">
                    <div class="content" style="overflow-y: scroll; max-height:90vh !important;">
                        <img src="@/assets/img/SingPassLogo.svg">
                        <h5><b>Verifying your identity with SingPass Face</b></h5>
                        <hr>
                        <br>
                        <p>The new SingPass face verification feature makes it easy for you to verify your identity by scanning your face. We are committed to protecting your data when you use the SingPass face verification feature:</p>
                        <br>
                        <b>1. Your consent is key</b>
                        <hr>
                        <p>You will be asked to give consent for the system to capture your facial images electronically and use that data to verify your identity for access to government digital services.</p>
                        <br>
                        <b>2. You are protected against fraud</b>
                        <hr>
                        <p>You will be asked to give consent for the system to capture your facial images electronically and use that data to verify your identity for access to government digital services.</p>
                        <br>
                        <b>3. Your data is encrypted</b>
                        <hr>
                        <p>All captured facial images are encrypted and protected with advanced technologies to prevent unauthorised access. Additional measures, such as security incident monitoring, are also put in place to ensure that your personal data is securely protected.</p>
                        <br>
                        <b>4. Storage of your image is time-limited</b>
                        <hr>
                        <p>The system will only keep the captured facial images for 30 days for audit purposes. Thereafter, the images will be deleted. There is no collection or storing of any other personal data from you on the system. Your data will not be used for any surveillance or commercial purposes.</p>
                        <p>If you do not wish to use the SingPass face verification feature, you may use other available way of authentication, such as SMS OTP.</p>
                        <br>
                        <small class="is-italic">Privacy Assurance as of 08 July 2020</small>
                        <br>
                        <br>
                        <br>
                        <br>
                        <br>
                    </div>
                </div>
            </div>
        </b-modal>
        </client-only>
    </section>
</template>

<script>
export default {
    // add head to include the generated client Javascript file => Script Tag
    head () {
        return {
            title: 'Identiface | Face Verify',
        }
    },
    created() {
    },
    mounted() {
        // Mounted is part of a lifecycle hook in VueJS. Once the DOM is ready, you can specify which functions to be run here.
        // This web app will retrieve the OAuth access token first on page serve
        this.token = ""
        this.nextURL = "/sample-app/final?token="
        this.alerts("Welcome!", "is-primary", 5000)

        if (process.client) { 
            window.SpfaceEvent = event => {

                var x = document.getElementsByTagName("PROGRESS")

                switch (event.type) {
                    case "aborted":
                        this.alerts("You've aborted the process.", "is-danger", 5000)
                        break
                    case "passed":
                    case "failed":
                        this.alerts("Click next to continue.", "is-info", 5000)
                        x[0].setAttribute("value", 100)
                        break
                    case "error":
                        this.alerts("Error: " + event.detail.reason, "is-danger", 5000)
                        break
                    case "ready":
                        x[0].setAttribute("value", 10)
                        this.alerts("Ready to begin face verification", "is-success", 3000)
                        break
                    case "unsupported":
                        this.alerts("Not supported!", "is-danger", 5000)
                        console.log(event)
                        break
                    case "permission":
                        this.alerts("Please enable permissions to continue", "is-info", 3000)
                        break
                    case "permission_denied":
                        this.alerts("You need to enable camera permissions to continue", "is-warning", 3000)
                        break
                    case "progress":

                        x[0].style.visibility = "visible"

                        var current = x[0].getAttribute("value")

                        var difference = event.detail.progress - current

                        for (let index = current; index < difference; index++) {
                            x[0].setAttribute("value", event.detail.progress)
                        }

                        if (event.detail.progress > 40 && event.detail.progress < 60) {
                            x[0].classList.remove("is-primary")
                            x[0].classList.add("is-info")
                        } else if (event.detail.progress > 60) {
                            x[0].classList.remove("is-info")
                            x[0].classList.add("is-success")
                        }
                        
                        break
                    case "feedback":
                        this.alerts(event.detail.reason, "is-info", 5000)
                    default:
                        console.log("sp-face" + event.detail.type + " " + event.type)
                }
            }
        }
    },
    data() {
        return {
            notice: "Thank you for signing up.",
            columnA: "column is-6 is-offset-3",
            label: "Validate NRIC",
            verified: false,
            token: "",
            nextURL: "/sample-app/final?token=",
            activeStep: 0,

            // data to send to the server
            service_id: "SingPass",
            user_id: "G2957839M",
            transaction_type: "SingPass onboarding",
            image: "",
            validated: false,

            // Loading icon
            isLoading: false,

            // Modal
            isCardModalActive: true,
            isPrivacyAssuranceActive: false,
        }
    },
    methods: {
        startVerify() {
            this.validateNRIC().then((token) => {
                if (token != "") {
                    this.validated = true
                    this.activeStep = 1
                    this.openLoading()
                }
            }).catch((error) => {
                if (error.response != undefined) {
                    var r = error.response.data
                    this.alerts(r.message, "is-danger", 2000)
                } else {
                    this.alerts("ERROR: Please check back in awhile.", "is-danger", 5000)
                }
            })
        },
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
        validateNRIC() {
            return new Promise((resolve, reject) => {
                this.token = ""
                let r = this.$axios.$post("/face/verify/token", {
                    "user_id": this.user_id, 
                    "service_id": this.service_id, 
                    "transaction_type": this.transaction_type
                }).then((tokenResponse) => {
                    if (tokenResponse.type == "error") {
                        this.alerts(tokenResponse.status + " " + tokenResponse.message.error_description, "is-danger", 5000)
                        this.notice = "Your NRIC is invalid or does not exist in the SingPass database."
                    } else {
                        var token = tokenResponse.token
                        this.token = tokenResponse.token
                        this.nextURL += this.token + "&user_id=" + this.user_id + "&service_id=" + this.service_id
                        this.alerts("NRIC validated!", "is-success", 2000)
                        this.notice = "We'll need to verify your identity now."

                        resolve(token)
                    }
                }).catch((error) => {
                    this.token = ""
                    if (error.response != undefined) {
                        var r = error.response.data
                        if (r.status == 429) {
                            this.alerts("Rate limit exceeded. Please try again in 1 minute.", "is-danger", 2000)
                            this.notice = "Try again later."
                        } else {
                            this.notice = "Your NRIC is invalid or does not exist in the SingPass database."
                            this.alerts("User doesn't exist in the SingPass database", "is-danger", 2000)
                        }
                    } else {
                        this.notice = "Please check back in awhile."
                        this.alerts("ERROR: Please check back in awhile.", "is-danger", 5000)
                    }
                })
            })
        },
        // Loading notification
        openLoading() {
            this.isLoading = true
            setTimeout(() => {
                this.isLoading = false
            }, 0.5 * 1000)
        },
    },
    
}
</script>

<style scoped>
    .m-1 {
        margin: 1rem 0 !important;
    }

    .card {
        height: 100%;
    }
</style>