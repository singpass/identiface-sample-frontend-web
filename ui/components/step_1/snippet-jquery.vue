<template>
    <section>
        <p>Append this after the <code>&lt;script&gt;</code> tags in Step 0:</p><br>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
<code class="language-javascript" ref="code">&lt;script type="text/javascript"&gt;
    // Important variables
    var baseURL = "{{baseURL}}";

    var token = "";
    var nric = "";

    function SpfaceEvent(event) {
        switch (event.type) {
            case "passed":
            case "failed":
                validateResult();
            case "error":
            case "unsupported":
            case "permission":
            case "permission_denied":
                console.warn("sp-face: " + event.type + " - " + event.detail.reason)
                break
            case "feedback":
                $("#message").append($('\
                    &lt;article class=&quot;message is-danger&quot;&gt;\
                        &lt;div class=&quot;message-header&quot;&gt;\
                            &lt;p&gt;Not authorised&lt;/p&gt;\
                        &lt;/div&gt;\
                        &lt;div class=&quot;message-body&quot;&gt;\
                            &lt;strong&gt;' + event.detail + '&lt;/strong&gt;\
                        &lt;/div&gt;\
                    &lt;/article&gt;\
                '));
            default:
                break;
        }
    }

    // Step 1
    $("#form-input").submit(function(event) {
        // Calls to start the service by getting the SDK Token
        event.preventDefault();
        nric = $("#nric").val();
        
        var payload = '{"service_id": "{{serviceID}}", "user_id": "' + nric + '", "pw":"{{pw}}"}';

        $.ajax({
            url: baseURL + "/face/verify/token",
            type: "POST",
            dataType: "json",
            contentType: "application/json",
            processData: false,
            data: payload,
            success: function(data2) {
                token = data2.token;
                // Inject the &lt;sp-face&gt; module here
                $("#sp-face-here").append($('\
                &lt;sp-face token=&quot;' + token + '&quot; base_url=&quot;{{singpassURL}}&quot;&gt;\
                    &lt;!-- Custom slots go in here --&gt;\
                    &lt;div id=&quot;sp-face-slots&quot;&gt;\
                        &lt;!-- Button for next action to validate results in Step 2 after user has completed the identification on Identiface --&gt;\
                        &lt;div slot=&quot;passed&quot; class=&quot;m-1&quot;&gt;\
                            &lt;!-- add a message to state that the SDK has passed your face verification --&gt;\
                            &lt;p&gt;Success!&lt;/p&gt;\
                        &lt;/div&gt;\
                    &lt;/div&gt;\
                &lt;/sp-face&gt;'));

                // Make the page look cleaner by removing the form
                $("#form-input").remove();
            },
            error: function(data3) {
                alert(JSON.stringify(data3));
            }
        });
    });

    // Place Step 2 here
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