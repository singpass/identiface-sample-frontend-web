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
                    <b-step-item :clickable="false">
                        <h1 class="title has-text-centered">-</h1>
                    </b-step-item>
                    <b-step-item step="1" label="Transfer details" :clickable="false">
                        <h1 class="title has-text-centered">Transfer details</h1>
                        <div class="card">
                            <div class="card-content">
                                <div class="content">
                                    <!-- For keying in of NRIC -->
                                    <form v-on:submit.prevent="startVerify">
                                        <b-field label="Key in your NRIC/FIN number to confirm">
                                            <b-input v-model="user_id" placeholder="S7012345A" required :disabled="validated"></b-input>
                                        </b-field><br>
                                        <b-button type="submit" class="is-primary" :disabled="!(!validated && this.user_id.length == 9)" v-on:click="startVerify">Next</b-button>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </b-step-item>
                    <b-step-item step="2" label="Verification" :clickable="false">
                        <h1 class="title has-text-centered">{{title}}</h1>
                        <div class="card">
                            <div class="card-content">
                                <div class="content" ref="verifyCard" v-if="padVerified" style="justify-content:center;">
                                    <img src="@/assets/img/tick.png" style="max-height: 30vh;">
                                    <h1>Success!</h1>
                                </div>
                                <div class="content" ref="verifyCard" v-if="!padVerified">
                                    <h4>Verify your identity with SingPass Face</h4>
                                    <img src="@/assets/img/singpass-icon-red.svg">
                                    <br>
                                    <a class="is-primary is-size-7"
                                        @click="isPrivacyAssuranceActive = true">
                                        Read Singpass Face Privacy Assurance
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
                                    logo="https://uat.api.singpass.gov.sg/assets/api-lib/img/singpass-icon-red.svg"
                                    show_countdown="false"
                                    kiosk_mode="false"
                                    :language="customLanguage"
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
                                                <b-button class="is-warning" id="allowedcam">Allow camera access</b-button><br><br>
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
                                                <b-button class="is-danger"><img src="https://uat.api.singpass.gov.sg/assets/api-lib/identiface/img/fv-inline-primary.svg"/></b-button>
                                            </div>
                                            <!-- User aborted the verification process -->
                                            <div slot="aborted" class="m-1">
                                                <p class="has-text-grey-dark">If you'd like to try again, please click the button below.<br></p>
                                            </div>
                                            <!-- Verification is sent to the server and is processing -->
                                            <div slot="progress" class="m-1">
                                                <p class="has-text-grey-dark">Verifying, please wait...<br></p>
                                                <progress class="progress is-small is-primary" max="100">15%</progress>
                                            </div>
                                            <!-- Passed verification -->
                                            <div slot="passed" class="m-1">
                                                <!-- Redirect to the next action on successful validation -->
                                                <p>You have successfully transferred.</p>
                                            </div>
                                            <!-- Failed verification -->
                                            <div slot="failed" class="m-1">
                                                <!-- Redirect to the next action on successful validation -->
                                                <p>You have failed verification. Retry?</p>
                                                <a class="button is-warning" href="/sample-app/employee-image">Retry</a>
                                            </div>
                                            <!-- Errors -->
                                            <div slot="error" class="m-1">
                                                <p class="has-text-grey-dark">Oh no, something went wrong. Please try again.<br></p>
                                                <a class="button is-warning" href="/sample-app/employee-image">Retry</a>
                                            </div>
                                        </div>
                                    </sp-face>
                                </div>
                            </div>
                        </div>
                    </b-step-item>
                    <b-step-item step="3" label="Upload image" :clickable="false">
                        <div :class="columnB" v-if="cameraState">
                            <div class="card">
                                <div class="card-image has-text-centered">
                                        <video ref="video" class="camera-stream" playsinline autoplay />
                                        <canvas ref="c" style="display:none;" :width="this.canvasWidth" :height="this.canvasHeight"></canvas>
                                </div>
                                <div class="card-content">
                                    <div class="content">
                                        <h1 ref="cameracontent" class="title"></h1>
                                        <button class="button is-dark" v-if="cameraReady" v-on:click="takeAPicture">Capture image</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div :class="columnC">
                <div class="card">
                    <div class="card-image" v-if="(this.user_id.length != 9) || (this.user_id.length == 9 && cameraState && !uploaded)" style="max-height:25vh;">
                        <figure class="image is-4by3">
                            <img ref="placeholder" src="@/assets/img/secure_login.svg" alt="Singpass face" style="max-height:20vh; margin-top:5vh;">
                            <br>
                        </figure>
                    </div>
                    <div class="card-content">
                        <form v-on:submit.prevent="cameraConfig">
                            <div class="content">
                                <h3 class="subtitle">{{notice}}<br></h3>
                                <br v-if="!uploaded">
                                <br v-if="!uploaded">
                                <hr>
                                <br v-if="!uploaded">
                                <p v-if="this.user_id.length == 9 && !cameraState && this.file == null && !uploaded">Choose to upload an image or take a selfie</p>
                                <b-field>
                                    <b-upload v-model="file"
                                        expanded
                                        drag-drop
                                        v-if="this.user_id.length == 9 && !cameraState && this.file == null && !uploaded">
                                        <section class="section">
                                            <div class="content has-text-centered">
                                                <p>
                                                    <b-icon
                                                        icon="upload"
                                                        size="is-large">
                                                    </b-icon>
                                                </p>
                                                <p>Click or drop your image here</p>
                                            </div>
                                        </section>
                                    </b-upload>
                                </b-field>
                                <b-button :type="this.buttontype" v-on:click="cameraConfig" v-if="this.user_id.length == 9">{{this.label}}</b-button>
                                <a class="button is-warning" v-if="verified" href="/sample-app/employee-image">Restart verification</a>
                            </div>
                        </form>
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
                        <p><b>Welcome to the Face Compare API demo.</b></p>
                        <p class="is-italic">Face Compare is a direct API endpoint to parse static images and get match results.</p>
                        <p>In this demo, we imagine a journey where an employee wants to submit a new employee card image.</p>
                        <br>
                        <a class="button is-primary" @click="isCardModalActive = false">Close</a>
                    </div>
                </div>
            </div>
        </b-modal>
        <client-only>
        <b-modal :active.sync="isPrivacyStatementActive" :width="640" scroll="keep">
            <div class="card">
                <div class="card-content">
                    <div class="content" style="overflow-y: scroll; max-height:90vh !important;">
                        <img src="@/assets/img/SingPassLogo.svg">
                        <h5><b>Verifying your identity with Singpass Face</b></h5>
                        <hr>
                        <br>
                        <p>The new Singpass face verification feature makes it easy for you to verify your identity by scanning your face. We are committed to protecting your data when you use the Singpass face verification feature:</p>
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
                        <p>If you do not wish to use the Singpass face verification feature, you may use other available way of authentication, such as SMS OTP.</p>
                        <br>
                        <small class="is-italic">Privacy Statement as of 08 July 2020</small>
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
    head () {
        return {
            title: 'Identiface | Face Compare',
        }
    },
    data() {
        return {
            uploaded: false,
            notice: "Your new employee card image must contain your own face.",
            columnA: "column is-6 is-offset-3",
            columnB: "column",
            columnC: "column",
            cameraState: false,
            buttontype: "is-primary",
            label: "Take a selfie instead",
            countdown: 5,
            verified: false,
            canvasWidth: 800,
            canvasHeight: 800,
            disableInput: false,
            cameraReady: false,


            activeStep: 1,
            title: "Verification",

            // uploadedFile
            file: null,

            // data to send to the server
            service_id: "SingPass",
            user_id: "",
            transaction_type: "Employee image onboarding (Identiface QuickStart)",
            image: "",
            validated: false,
            padVerified: false,

            // Modal
            isCardModalActive: true,
            isPrivacyStatementActive: false,


            // Loading icon
            isLoading: false,
        }
    },
    watch: {
        turnOnCamera: function() {
            if (this.turnOnCamera) {
                this.canvasWidth = this.$refs.video.clientWidth
                this.canvasHeight = this.$refs.video.clientHeight
                console.log(this.$refs.video.clientHeight)
            }
        },
        file: function() {
            if (this.file != null) {
                this.disableInput = true
                new Promise((resolve, reject) => {
                    var img = this.file
                    // Check if file size > 500kb (if too large, server will reject)
                    if (img.size < 2000000) {
                        // Check if file type is jpeg or png
                        if (img.type != "image/jpeg" && img.type != "image/png") {
                            this.alerts("File is not in JPEG or PNG format!", "is-danger", 5000)
                            reject("File is not in JPEG or PNG format")
                            this.file = null
                        } else {
                            const reader = new FileReader()
                            reader.readAsDataURL(img)
                            reader.onload = () => resolve(reader.result)
                            reader.onerror = error => reject(error)
                        }
                    } else {
                        this.alerts("File is too large!", "is-danger", 5000)
                        reject("File is too large")
                        this.file = null
                    }
                }).then((img) => {
                    this.uploaded = true
                    this.verifyPerson(img)
                }).catch((error) => {
                    console.log(error)
                })
            }
        }
    },
    mounted() {
        this.token = ""
        this.notice = "Let's step up your authentication."
        
        if (process.client) { 

            // this.useLanguage()
            window.SpfaceEvent = event => {

                var x = document.getElementsByTagName("PROGRESS")

                switch (event.type) {
                    case "aborted":
                        this.alerts("You've aborted the process.", "is-danger", 5000)
                        break
                    case "passed":
                        x[0].setAttribute("value", 100)
                        this.validateResult()
                        break
                    case "failed":
                        x[0].classList.remove("is-success")
                        x[0].classList.add("is-danger")
                        x[0].setAttribute("value", 100)
                        this.alerts("Error: " + event.detail.reason, "is-danger", 5000)
                        this.validateResult()
                        break
                    case "error":
                        this.alerts("Error: " + event.detail.reason, "is-danger", 5000)
                        this.validateResult()
                        break
                    case "ready":
                        x[0].setAttribute("value", 10)
                        this.alerts("Ready to begin face verification", "is-success", 3000)
                        break
                    case "unsupported":
                        this.alerts("Not supported!", "is-danger", 5000)
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
    methods: {
        alerts(message, type, duration) {
            this.$buefy.toast.open({
                duration: duration,
                message: message,
                type: type,
                queue: false,
            })
        },
        startTimer() {
            this.countdown--
        },
        startVerify() {
            this.validateNRIC().then((token) => {
                if (token != "") {
                    this.validated = true
                    this.activeStep = 2
                    this.openLoading()
                }
            }).catch((error) => {
                if (error.response != undefined) {
                    var r = error.response.data
                    this.alerts(r.message, "is-danger", 2000)
                } else {
                    this.alerts("ERROR: Service is offline", "is-danger", 5000)
                }
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
                        console.log(error)
                        this.notice = "Please check back in a minute."
                        this.alerts("ERROR: Please check back in awhile.", "is-danger", 5000)
                    }
                })
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
                        this.padVerified = true
                        this.notice = "Proof of presence successful! Please upload an image of yourself."
                        this.title = "Success!"
                        this.activeStep = 3
                    } else {
                        this.alerts("Failed verification", "is-danger", 5000)
                    }
                } else {
                    var message = vResponse.message
                    console.log(vResponse)
                    this.alerts(message.error_description, "is-danger", 5000)
                }
            }).catch((error) => {

                if (error.response != undefined) {
                    var r = error.response.data
                    var message = r.message.error_description
                    this.alerts(message, "is-danger", 2000)
                } else {
                    this.alerts("ERROR: Please check back in awhile", "is-danger", 5000)
                }
            })
        },
        // Loading notification
        openLoading() {
            this.isLoading = true
            setTimeout(() => {
                this.isLoading = false
            }, 0.5 * 1000)
        },
        cameraConfig() {
            if (this.verified) {
                this.$router.push('/final')
            } else {
                if (!this.cameraState) {
                    this.uploaded = false
                    this.countdown = 5
                    this.turnOnCamera()
                    this.disableInput = true
                    this.cameraState = true
                    this.columnA = "column"
                    this.columnB = "column is-two-thirds"
                    this.notice = "Position your head in the centre of the frame, or choose to upload your image instead."
                } else {
                    this.turnOffCamera()
                    this.disableInput = false
                    this.cameraState = false
                    this.columnA = "column is-4 is-offset-4"
                    this.columnB = "column"
                    this.cameraReady = false
                }
            }
            
        },
        takeAPicture() {
            this.canvasWidth = this.$refs.video.clientWidth
            this.canvasHeight = this.$refs.video.clientHeight
            this.captureImage().then((response) => {
                this.notice = "Hold on..."
                this.verifyPerson(response)
            }).catch((error) => {
                this.alerts(error)
                console.log(error)
            })
        },
        captureImage() {
            if (this.cameraState) {
                return new Promise((resolve) => {
                    this.alerts("Taking a picture", "is-primary", 100)

                    var canvas = this.$refs.c
                    
                    console.log(canvas.clientHeight)

                    canvas.getContext("2d").drawImage(this.$refs.video, 0, 0, this.$refs.video.clientWidth, this.$refs.video.clientHeight);

                    // img is the base64 encode of jpeg
                    var img = canvas.toDataURL("image/jpeg");
                    console.log(img)
                    this.uploaded = true
                    this.image = img
                    this.alerts("Picture taken", "is-info", 1000)
                    resolve(img)
                })
            }
        },
        turnOnCamera () {
            
            this.label = "Turn off camera"
            this.buttontype = "is-danger"
            navigator.mediaDevices.getUserMedia({ video: true }).then(mediaStream => {
                this.$refs.video.srcObject = mediaStream
                this.$refs.video.play()
                var number = this.countdown
                this.cameraReady = true
            }).catch((error) => {
                this.alerts(error, "is-danger", 5000)
                this.cameraConfig()
                this.cameraReady = false
            })
        },
        turnOffCamera() {
            this.label = "Retry image capture"
            this.buttontype = "is-warning"

            try {
                const tracks = this.$refs.video.srcObject.getTracks()
                tracks.map(track => track.stop())
            } catch {
                console.log("Camera not turned on or doesn't exist.")
            } finally {
                this.notice = "Your new employee card image must be an image of yourself."
                this.cameraReady = false
            }
        },
        verifyPerson(img) {
            this.notice = "Verifying for " + this.user_id
            try {
                let req = this.$axios.$post("/face/compare", {
                        "service_id": this.service_id, 
                        "user_id": this.user_id, 
                        "image": img
                    }).then((msResponse) => {
                    var type = msResponse.type

                    if (type === "success") {
                        var score = msResponse.score
                        var scorePercentage = score * 100

                        var minimumThreshold = 0.70

                        if (score > minimumThreshold) {
                            this.alerts("Verified!", "is-success", 5000)
                            this.notice = "Success! Match result: " + Math.ceil(scorePercentage) + "%"
                            this.label = "Next"
                            this.buttontype = "is-dark"
                            this.verified = true
                        } else {
                            this.alerts("Not authorised! Match result: " + Math.ceil(scorePercentage) + "%", "is-danger", 5000)
                            this.notice = "Identity cannot be verified successfully. If you are " + this.user_id + ", please try again."
                        }
                    } else {
                        var status = msResponse.status
                        var message = msResponse.message

                        this.alerts("Error " + status + ", " + message, "is-danger", 5000)
                        this.notice = "Error: " + message + ". Click the button below to retry."
                    }
                }).catch((error) => {
                    this.notice = "Error: "
                    if (error.response != undefined) {
                        var r = error.response.data
                        this.alerts("Error: " + r.message, "is-danger", 2000)
                        this.notice += r.message
                    } else {
                        this.notice = "Please check back in awhile."
                        this.alerts("ERROR: Please check back in awhile.", "is-danger", 5000)
                    }
                })
            } catch {
                this.alerts("Something went terribly wrong.", "is-danger", 5000)
            } finally {
                if (this.file == null) {
                    this.cameraConfig()
                } else {
                    this.file = null
                }
            }
            
        }
    },
    
}
</script>

<style scoped>

    video {
        /* max-width: 1000px;
        max-height: 1000px; */
        overflow:hidden !important;
    }

    .card {
        height: 100%;
    }
</style>