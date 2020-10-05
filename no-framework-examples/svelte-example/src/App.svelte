<script>
    import { Col, Container, Row, Jumbotron, Button, Form, FormText, FormGroup, Label, Input } from "sveltestrap";

    let startFace = false;
    let buttonText = "Start";
    let buttonColour = "primary";

    let baseURL = "http://localhost:9000";
    let user_id = "";
    let service_id = "SingPass";

    let userValidated = false;
    let token = "";

    let faceScanningPassed = true;
    
    function start() {
        startFace = !startFace;

        if (!startFace) {
            buttonText = "Start";
            buttonColour = "primary";
        } else {
            buttonText = "Cancel";
            buttonColour = "danger";
        }
        
    }

    async function getSessionToken() {
        const payload = JSON.stringify({
            "service_id": service_id,
            "user_id": user_id
        });

        const res = await fetch(baseURL + "/face/verify/token", {
            method: 'POST',
            headers: {
                "Accept": "application/json",
                "Content-Type": "application/json"
            },
            body: payload
        });

        const json = await res.json();

        if (json.type == "success") {
            userValidated = true;
            token = json.token;
        } else {
            alert(json.message);
        }

        const valid = await userValidated == true;
        
        if (valid) {
            const append = false;
            const el = document.getElementById("sp-face-here");
            // create the <sp-face> element
            var spface = document.createElement("SP-FACE");
            spface.setAttribute("token", token);
            spface.setAttribute("base_url", "https://stg-bio-stream.singpass.gov.sg");
            spface.innerHTML = '\
            <!-- Custom slots go in here -->\
                <div id="sp-face-slots">\
                    <div slot="passed" class="m-1">\
                    </div>\
                    <!-- Failed verification -->\
                    <div slot="failed" class="m-1">\
                    </div>\
                </div>';
            el.appendChild(spface);
        }
    }

    async function validate() {
        const validatePayload = JSON.stringify({
            "service_id": service_id,
            "user_id": user_id
            "token": token
        });
        const res = await fetch(baseURL + "/face/verify/validate", {
            method: 'POST',
            headers: {
                "Accept": "application/json",
                "Content-Type": "application/json"
            },
            body: validatePayload
        });

        const json = await res.json();

        if (json.type == "success") {
            var innerHTML = "";
            if (json.is_passed) {
                innerHTML = '\
            <div class="alert alert-success" role="alert">\
                You\'ve passed the face matching!\
            </div>';
            } else {
                innerHTML = '\
            <div class="alert alert-danger" role="alert">\
                You\'ve failed the face matching\
            </div>';
            }
            document.getElementsByTagName("SP-FACE")[0].innerHTML = innerHTML;
        } else {
            document.getElementsByTagName("SP-FACE")[0].innerHTML = '\
            <div class="alert alert-success" role="alert">\
                ' + json.message + '\
            </div>';
            alert(json.message);
        }
    }

    window.SpfaceEvent = event => {
        switch (event.type) {
            case "passed":
            case "failed":
                validate();
                break;
            default:
                break;
        }
    }
</script>

<main>
    <Jumbotron style="height: 100%;" class="bg-light">
        <Row class="align-items-center">
        <Col>
            <h1 class="display-3">Hello, Identiface yourself.</h1>
            {#if !startFace}
            <p class="lead">
                Try out Face Verify with Svelte. It's easy and simple.
            </p>
            <hr class="my-2" />
            <p>
                Click to begin.
            </p>
            <p class="lead">
                <Button color="{buttonColour}" on:click={start}>{buttonText}</Button>
            </p>
            {:else}
            <div class="text-left">
            <hr>
                <Row class="justify-content-md-center">
                    <Col class="col-md-5">
                        {#if !userValidated}
                        <form on:submit|preventDefault="{getSessionToken}">
                            <FormGroup>
                                <p>Hello, <b>{user_id === "" ? "key in your NRIC/FIN to begin" : user_id}</b>.</p>
                                <Label for="exampleEmail">NRIC/FIN Number</Label>
                                <Input type="text" name="user_id" bind:value="{user_id}" placeholder="S0000003C" />
                            </FormGroup>
                            <FormGroup>
                                <Row class="justify-content-md-center">
                                    <Col class="col-md-2">
                                        <p class="lead">
                                            <Button disabled={user_id.length != 9} type="submit" color="{user_id.length != 9 ? "secondary" : "success"}">Verify</Button>
                                        </p>
                                    </Col>
                                    <Col class="col-md-2">
                                        <p class="lead">
                                            <Button color="{buttonColour}" on:click={start}>{buttonText}</Button>
                                        </p>
                                    </Col>
                                </Row>
                            </FormGroup>
                        </form>
                        {:else}
                            <div id="sp-face-here"></div>
                        {/if}
                    </Col>
                </Row>
            </div>
            {/if}
            
            
            </Col>
        </Row>
    </Jumbotron>
</main>

<style>
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
</style>