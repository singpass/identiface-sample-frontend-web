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
    &lt;script src=&quot;https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;</code>
        </pre>
        <br>
        <hr>
        <p>Add these to your <code>&lt;body></code> tag:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy2" expanded>Copy</button>
<script type="text/plain" class="language-markup" ref="body"><body>
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
                                <div id="message">
                                    <!-- insert message -->
                                </div>
                                <form id="form-input">
                                    <input class="input" id="nric" type="text" placeholder="Fill in your NRIC number here">
                                    <br>
                                    <br>
                                    <button type="submit" class="button is-info">Submit</button>
                                </form>
                                <!-- This will be where the <sp-face> component will be inserted -->
                                <div id="sp-face-here">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
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