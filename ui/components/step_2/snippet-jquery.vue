<template>
    <section>
        <button class="button is-dark is-small is-outlined" v-on:click="copy" expanded>Copy</button>
        <pre>
            <code class="language-javascript" ref="code">
            // Step 2, place this outside the #form-input.submit() function
            function validateResult() {
                var payload = '{"service_id": "{{serviceID}}", "user_id": "' + nric + '", "pw": "{{pw}}", "token": "' + token + '"}';

                $.ajax({
                    url: baseURL + "/face/verify/validate",
                    type: "POST",
                    dataType: "json",
                    contentType: "application/json",
                    processData: false,
                    data: payload,
                    success: function(data) {
                        var isPassed = data.is_passed;
                        var score = Math.ceil(data.score * 100);
                        
                        if (isPassed) {
                            $("#message").append($('\
                                &lt;article class=&quot;message is-success&quot;&gt;\
                                    &lt;div class=&quot;message-header&quot;&gt;\
                                        &lt;p&gt;Success!&lt;/p&gt;\
                                    &lt;/div&gt;\
                                    &lt;div class=&quot;message-body&quot;&gt;\
                                        &lt;strong&gt;You\'ve passed verification!&lt;/strong&gt;\
                                    &lt;/div&gt;\
                                &lt;/article&gt;\
                            '));
                        } else {
                            $("#message").append($('\
                                &lt;article class=&quot;message is-danger&quot;&gt;\
                                    &lt;div class=&quot;message-header&quot;&gt;\
                                        &lt;p&gt;Failed verification&lt;/p&gt;\
                                    &lt;/div&gt;\
                                    &lt;div class=&quot;message-body&quot;&gt;\
                                        &lt;strong&gt;You\'ve failed verification!&lt;/strong&gt;\
                                    &lt;/div&gt;\
                                &lt;/article&gt;\
                            '));
                        }
                        // Remove div after completing
                        $("#sp-face-here").remove();
                    },
                    error: function(data) {
                        alert(JSON.stringify(data));
                    }
                });
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