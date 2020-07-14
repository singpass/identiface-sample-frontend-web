<template>
    <section>
        <p>Append this inside the <code>&lt;script&gt;</code> tags in Step 0:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
<code class="language-javascript" ref="code">   async function getSessionToken() {
        const payload = JSON.stringify({
            &quot;service_id&quot;: service_id,
            &quot;user_id&quot;: user_id,
            &quot;pw&quot;: pw
        });

        const res = await fetch(baseURL + &quot;/face/verify/token&quot;, {
            method: 'POST',
            headers: {
                &quot;Accept&quot;: &quot;application/json&quot;,
                &quot;Content-Type&quot;: &quot;application/json&quot;
            },
            body: payload
        });

        const json = await res.json();

        if (json.type == &quot;success&quot;) {
            userValidated = true;
            token = json.token;
        } else {
            alert(json.message);
        }

        const valid = await userValidated == true;
        
        if (valid) {
            const append = false;
            const el = document.getElementById(&quot;sp-face-here&quot;);
            // create the &lt;sp-face&gt; element
            var spface = document.createElement(&quot;SP-FACE&quot;);
            spface.setAttribute(&quot;token&quot;, token);
            spface.setAttribute(&quot;base_url&quot;, &quot;https://stg-bio-stream.singpass.gov.sg&quot;);
            spface.innerHTML = '\
            &lt;!-- Custom slots go in here --&gt;\
                &lt;div id=&quot;sp-face-slots&quot;&gt;\
                    &lt;div slot=&quot;passed&quot; class=&quot;m-1&quot;&gt;\
                    &lt;/div&gt;\
                    &lt;!-- Failed verification --&gt;\
                    &lt;div slot=&quot;failed&quot; class=&quot;m-1&quot;&gt;\
                    &lt;/div&gt;\
                &lt;/div&gt;';
            el.appendChild(spface);
        }
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