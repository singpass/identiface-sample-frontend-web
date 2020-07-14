<template>
<!-- Change all links and environment variables in data() below so that it will dynamically change in the snippets -->
    <section>
        <p><b>Ensure that you have <code>npm</code> installed.</b></p><br>
        <p>Navigate to a new project folder, and run the following commands on your UNIX terminal:</p><br>
        <pre style="max-height:20vh;">
<code class="language-markup" ref="code">npx degit sveltejs/template svelte-face-verify
cd svelte-face-verify

npm install
npm install --save sveltestrap svelte
npm run dev
</code>
        </pre>
        <br>
        <p>Sveltestrap uses the Bootstrap CSS framework.</p><br>
        <hr>
        <p>In the <code>App.svelte</code> file, replace the contents of your file to the code below:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy1" expanded>Copy</button>
        <pre style="max-height:70vh;">
<code class="language-markup" ref="body">&lt;script&gt;
    import { Col, Container, Row, Jumbotron, Button, Form, FormText, FormGroup, Label, Input } from &quot;sveltestrap&quot;;

    let startFace = false;
    let buttonText = &quot;Start&quot;;
    let buttonColour = &quot;primary&quot;;

    let baseURL = &quot;{{baseURL}}&quot;;
    let pw = &quot;{{pw}}&quot;;
    let user_id = &quot;&quot;;
    let service_id = &quot;{{serviceID}}&quot;;

    let userValidated = false;
    let token = &quot;&quot;;

    let faceScanningPassed = true;
    
    function start() {
        startFace = !startFace;

        if (!startFace) {
            buttonText = &quot;Start&quot;;
            buttonColour = &quot;primary&quot;;
        } else {
            buttonText = &quot;Cancel&quot;;
            buttonColour = &quot;danger&quot;;
        }
        
    }
// Steps 1 & 2
&lt;/script&gt;
&lt;main&gt;
    &lt;Jumbotron style=&quot;height: 100%;&quot; class=&quot;bg-light&quot;&gt;
        &lt;Row class=&quot;align-items-center&quot;&gt;
        &lt;Col&gt;
            &lt;h1 class=&quot;display-3&quot;&gt;Hello, Identiface yourself.&lt;/h1&gt;
            {#if !startFace}
            &lt;p class=&quot;lead&quot;&gt;
                Try out Face Verify with Svelte. It's easy and simple.
            &lt;/p&gt;
            &lt;hr class=&quot;my-2&quot; /&gt;
            &lt;p&gt;
                Click to begin.
            &lt;/p&gt;
            &lt;p class=&quot;lead&quot;&gt;
                &lt;Button color=&quot;{buttonColour}&quot; on:click={start}&gt;{buttonText}&lt;/Button&gt;
            &lt;/p&gt;
            {:else}
            &lt;div class=&quot;text-left&quot;&gt;
            &lt;hr&gt;
                &lt;Row class=&quot;justify-content-md-center&quot;&gt;
                    &lt;Col class=&quot;col-md-5&quot;&gt;
                        {#if !userValidated}
                        &lt;form on:submit|preventDefault=&quot;{getSessionToken}&quot;&gt;
                            &lt;FormGroup&gt;
                                &lt;p&gt;Hello, &lt;b&gt;{user_id === &quot;&quot; ? &quot;key in your NRIC/FIN to begin&quot; : user_id}&lt;/b&gt;.&lt;/p&gt;
                                &lt;Label for=&quot;exampleEmail&quot;&gt;NRIC/FIN Number&lt;/Label&gt;
                                &lt;Input type=&quot;text&quot; name=&quot;user_id&quot; bind:value=&quot;{user_id}&quot; placeholder=&quot;S0000003C&quot; /&gt;
                            &lt;/FormGroup&gt;
                            &lt;FormGroup&gt;
                                &lt;Row class=&quot;justify-content-md-center&quot;&gt;
                                    &lt;Col class=&quot;col-md-2&quot;&gt;
                                        &lt;p class=&quot;lead&quot;&gt;
                                            &lt;Button disabled={user_id.length != 9} type=&quot;submit&quot; color=&quot;{user_id.length != 9 ? &quot;secondary&quot; : &quot;success&quot;}&quot;&gt;Verify&lt;/Button&gt;
                                        &lt;/p&gt;
                                    &lt;/Col&gt;
                                    &lt;Col class=&quot;col-md-2&quot;&gt;
                                        &lt;p class=&quot;lead&quot;&gt;
                                            &lt;Button color=&quot;{buttonColour}&quot; on:click={start}&gt;{buttonText}&lt;/Button&gt;
                                        &lt;/p&gt;
                                    &lt;/Col&gt;
                                &lt;/Row&gt;
                            &lt;/FormGroup&gt;
                        &lt;/form&gt;
                        {:else}
                            &lt;div id=&quot;sp-face-here&quot;&gt;&lt;/div&gt;
                        {/if}
                    &lt;/Col&gt;
                &lt;/Row&gt;
            &lt;/div&gt;
            {/if}
            
            
            &lt;/Col&gt;
        &lt;/Row&gt;
    &lt;/Jumbotron&gt;
&lt;/main&gt;

&lt;style&gt;
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
        height: 100%;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
&lt;/style&gt;
</code>
        </pre>
        <br>
        <hr>
        <p>Lastly, in the <code>index.html</code> file, add this to your <code>&lt;head></code> tag:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy2" expanded>Copy</button>
        <pre style="max-height:5vh;">
<code class="language-markup" ref="head">&lt;script src="{{scriptURL}}"&gt;&lt;/script&gt;
</code>
        </pre>
    </section>
</template>

<script>
export default {
    props: {
        baseURL: String,
        pw: String,
        scriptURL: String,
        serviceID: String,
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
            navigator.clipboard.writeText(this.$refs.body.textContent)
        },
        copy2() {
            this.alerts("Copied to clipboard!", "is-info", 3000)
            navigator.clipboard.writeText(this.$refs.head.textContent)
        }
    }
}
</script>