<template>
    <section class="section">
        <div class="columns">
            <div :class="columnB" v-if="cameraState">
                <div class="card">
                    <div class="card-image has-text-centered">
                        <div ref="divH" class="camera-modal">
                            <video ref="video" class="camera-stream" />
                            <canvas ref="c" style="display:none;" :width="this.canvasWidth" :height="this.canvasHeight"></canvas>
                        </div>
                    </div>
                    <div class="card-content">
                        <div class="content">
                            <h1 ref="cameracontent" class="title"></h1>
                            <button class="button is-dark" v-if="cameraReady" v-on:click="takeAPicture">Capture image</button>
                        </div>
                    </div>
                </div>
            </div>
            <div :class="columnA">
                <div class="card">
                    <div class="card-image" v-if="(this.user_id.length != 9) || (this.user_id.length == 9 && cameraState && !uploaded)" style="max-height:25vh;">
                        <figure class="image is-4by3">
                            <img ref="placeholder" src="@/assets/img/secure_login.svg" alt="SingPass face" style="max-height:20vh; margin-top:5vh;">
                            <br>
                        </figure>
                    </div>
                    <div class="card-content">
                        <form v-on:submit.prevent="cameraConfig">
                            <div class="content">
                                <h3 class="subtitle">{{notice}}<br></h3>
                                <b-field label="Scan or fill up your NRIC/FIN number" v-if="!uploaded">
                                    <b-input v-model="user_id" placeholder="NRIC/FIN number" required :disabled="disableInput"></b-input>
                                </b-field>
                                <a class="has-text-danger is-size-7" @click="user_id = 'G2834561K'" v-if="this.user_id == 'G2957839M' && !uploaded"><u>Click to set a dummy NRIC persona that will fail verification</u></a>
                                <a class="has-text-info is-size-7" @click="user_id = 'G2957839M'" v-if="(this.user_id == '' || this.user_id == 'G2834561K') && !uploaded"><u>Click to set a dummy NRIC persona that will pass verification</u></a>
                                <br v-if="!uploaded">
                                <br v-if="!uploaded">
                                <hr>
                                <br v-if="!uploaded">
                                <div v-if="(this.user_id.length != 9) || (this.user_id.length == 9 && cameraState && !uploaded)">
                                    <p>We will verify your image using the <b>SingPass Face Verification service</b>.</p>
                                    <a @click="isPrivacyStatementActive = true"><u>Read privacy statement</u></a>
                                    <br><br>
                                    <a class="button is-primary" href="/">Use another way</a>
                                </div>
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
        </div>
        <b-modal :active.sync="isCardModalActive" :width="640" scroll="keep">
            <div class="card">
                <div class="card-content">
                    <div class="content">
                        <p><b>Welcome to the Face Compare API demo.</b></p>
                        <p class="is-italic">Face Compare is a direct API endpoint to parse static images and get match results.</p>
                        <p>In this demo, we imagine a journey where an employee wants to submit a new employee card image.</p>
                        <p>We will be using a dummy NRIC persona <code>G2957839M</code>, which will return an always <code>true</code> result when verifying the identity, regardless of the face.</p>
                        <p>If you'd like to test a flow that will <b>fail</b> authentication, please replace the dummy NRIC persona to <code>G2834561K</code>. This will return a <code>false</code> match result.</p>
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
            columnA: "column is-4 is-offset-4",
            columnB: "column",
            cameraState: false,
            buttontype: "is-primary",
            label: "Take a selfie instead",
            countdown: 5,
            verified: false,
            canvasWidth: 800,
            canvasHeight: 800,
            disableInput: false,
            cameraReady: false,

            // uploadedFile
            file: null,

            // data to send to the server
            service_id: "serviceA",
            user_id: "",
            transaction_type: "",
            image: "",
            
            // environmentURLs
            backendPass: "ndi-api",

            // Modal
            isCardModalActive: true,
            isPrivacyStatementActive: false,
        }
    },
    watch: {
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
                    canvas.getContext("2d").drawImage(this.$refs.video, 0, 0, this.canvasWidth, this.canvasHeight);

                    // img is the base64 encode of jpeg
                    var img = canvas.toDataURL("image/jpeg");
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
                this.canvasWidth = 1000
                this.canvasHeight = 1000 / 4 * 3
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
                        "image": img, 
                        "pw": this.backendPass
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
                        this.notice = "Verification service is offline."
                        this.alerts("ERROR: Service is offline", "is-danger", 5000)
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
        max-width: 1000px;
        max-height: 1000px;
        overflow:hidden !important;
    }

    .card {
        height: 100%;
    }
</style>