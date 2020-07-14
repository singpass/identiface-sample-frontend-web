<template>
<!-- Change all links and environment variables in data() below so that it will dynamically change in the snippets -->
    <section>
        <p>In a new HTML file, add these to your <code>&lt;head></code> tag:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy1" expanded>Copy</button>
        <pre style="max-height:25vh;">
<code class="language-markup" ref="head">&lt;head&gt;
    &lt;meta charset=&quot;utf-8&quot;&gt;
    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;
    &lt;title&gt;Identiface HTML Example&lt;/title&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bulma@0.8.2/css/bulma.min.css&quot;&gt;
    &lt;script defer src=&quot;https://use.fontawesome.com/releases/v5.3.1/js/all.js&quot;&gt;&lt;/script&gt;
    
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/vue&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/axios/0.19.2/axios.min.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;</code>
        </pre>
        <br>
        <hr>
        <p>Add these to your <code>&lt;body></code> tag, and remove the escape character in <code>&lt;article&gt;</code>:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy2" expanded>Copy</button>
<script type="text/plain" class="language-markup" ref="body"><body>
    <div id="app">
        <section class="hero is-fullheight">
            <div class="hero-body">
                <div class="container">
                    <h1 class="title has-text-centered">
                        Hello, identiface yourself.
                    </h1>
                    <div class="columns">
                        <div class="column is-6 is-offset-3">
                            <div class="card">
                                <div class="card-content">
                                    <div id="messages">
                                        <!-- insert messages -->
                                        <article v-bind:class="[message_colour]">
                                            <div class="message-body">
                                                <!-- Remove the escape character below -->
                                                {\{ message }}
                                            </div>
                                        </article>
                                    </div>
                                    <form v-bind:class="[form_hidden ? 'is-hidden' : '']" id="form-input" @submit.preventDefault="startVerify">
                                        <input class="input" id="nric" type="text" v-model="user_id" placeholder="Fill in your NRIC number here">
                                        <br>
                                        <br>
                                        <button type="submit" class="button is-primary">Submit</button>
                                    </form>
                                    <!-- This will be where the <sp-face> component will be inserted -->
                                    <div v-bind:class="[!form_hidden ? 'is-hidden' : '']" id="sp-face-here">
                                        <sp-face v-if="token != ''" v-bind:token="[token]" v-bind:base_url="[singpassURL]">
                                            <!-- Custom slots go in here -->
                                            <div id="sp-face-slots">
                                                <!-- No Camera -->
                                                <div slot="no_camera">
                                                    <br>
                                                    <p class="has-text-danger">You don't have a camera on your device.<br></p>
                                                </div>
                                                <!-- Unsupported browser -->
                                                <!-- Permissions check -->
                                                <div slot="grant_permission">
                                                    <br>
                                                    <p class="has-text-grey-dark">Before we proceed, please allow us to access your camera.<br></p>
                                                </div>
                                                <br>
                                                <div slot="grant_button">
                                                    <button class="button is-warning">Allow camera access</button>
                                                </div>
                                                <!-- Permissions denied -->
                                                <div slot="permission_denied">
                                                    <br>
                                                    <p class="has-text-danger">You've denied permissions. If you would like to continue with the verification, please refresh the page.<br></p>
                                                </div>
                                                <!-- UI has access to camera and is ready to begin verification -->
                                                <div slot="ready">
                                                    <br>
                                                    <p class="has-text-grey-dark">Thank you. Please proceed to verify your identity.<br></p>
                                                </div>
                                                <br>
                                                <div slot="button">
                                                    <button class="button is-primary">Scan my face</button>
                                                </div>
                                                <!-- User aborted the verification process -->
                                                <div slot="aborted">
                                                    <br>
                                                    <p class="has-text-grey-dark">If you'd like to try again, please click the button below.<br></p>
                                                </div>
                                                <!-- Verification is sent to the server and is processing -->
                                                <div slot="progress">
                                                    <br>
                                                    <p class="has-text-grey-dark">Verifying, please wait...<br></p>
                                                    <progress class="progress is-primary" max="100"></progress>
                                                </div>
                                                <!-- Passed verification -->
                                                <div slot="passed">
                                                </div>
                                                <!-- Failed verification -->
                                                <div slot="failed">
                                                </div>
                                                <!-- Errors -->
                                                <div slot="error">
                                                    <br>
                                                    <p class="has-text-grey-dark">Oh no, something went wrong. Please try again.<br></p>
                                                    <a class="button is-warning" href="/html_sdk">Retry</a>
                                                </div>
                                            </div>
                                        </sp-face>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</body></script>
        <br>
        <hr>
        <p>Lastly, add this snippet after the <code>&lt;body></code> tag. This links you to our Identiface SDK script.</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy3" expanded>Copy</button>
        <pre style="max-height:10vh;">
<code class="language-javascript" ref="script">&lt;script src=&quot;{{scriptURL}}&quot;&gt;&lt;/script&gt;</code>
        </pre>
    </section>
</template>

<script>
export default {
    props: {
        baseURL: String,
        pw: String,
        scriptURL: String,
    },
    data() {
        return {
            rawHTML: ``
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
        copy1() {
            this.alerts("Copied to clipboard!", "is-info", 3000)
            navigator.clipboard.writeText(this.$refs.head.textContent)
        },
        copy2() {
            this.alerts("Copied to clipboard!", "is-info", 3000)
            navigator.clipboard.writeText(this.$refs.body.textContent)
        },
        copy3() {
            this.alerts("Copied to clipboard!", "is-info", 3000)
            navigator.clipboard.writeText(this.$refs.script.textContent)
        },
    }
}
</script>