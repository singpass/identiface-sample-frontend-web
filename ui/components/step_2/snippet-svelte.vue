<template>
    <section>
        <p>Append this inside the <code>&lt;script&gt;</code> tags in Step 0:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
<code class="language-javascript" ref="code">   async function validate() {
        const validatePayload = JSON.stringify({
            &quot;service_id&quot;: service_id,
            &quot;user_id&quot;: user_id,
            &quot;pw&quot;: &quot;ndi-api&quot;,
            &quot;token&quot;: token
        });
        const res = await fetch(baseURL + &quot;/face/verify/validate&quot;, {
            method: 'POST',
            headers: {
                &quot;Accept&quot;: &quot;application/json&quot;,
                &quot;Content-Type&quot;: &quot;application/json&quot;
            },
            body: validatePayload
        });

        const json = await res.json();

        if (json.type == &quot;success&quot;) {
            var innerHTML = &quot;&quot;;
            if (json.is_passed) {
                innerHTML = '\
            &lt;div class=&quot;alert alert-success&quot; role=&quot;alert&quot;&gt;\
                You\'ve passed the face matching!\
            &lt;/div&gt;';
            } else {
                innerHTML = '\
            &lt;div class=&quot;alert alert-danger&quot; role=&quot;alert&quot;&gt;\
                You\'ve failed the face matching\
            &lt;/div&gt;';
            }
            document.getElementsByTagName(&quot;SP-FACE&quot;)[0].innerHTML = innerHTML;
        } else {
            document.getElementsByTagName(&quot;SP-FACE&quot;)[0].innerHTML = '\
            &lt;div class=&quot;alert alert-success&quot; role=&quot;alert&quot;&gt;\
                ' + json.message + '\
            &lt;/div&gt;';
            alert(json.message);
        }
    }

    window.SpfaceEvent = event =&gt; {
        switch (event.type) {
            case &quot;passed&quot;:
            case &quot;failed&quot;:
                validate();
                break;
            default:
                break;
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