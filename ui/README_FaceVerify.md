# Identiface Face Verify

> ### TL;DR
> 
> First, to use the SDK, you need to get the OAuth 2.0 Access Token from the `/face/oauth/token/` endpoint using your Client ID & Secret.
> 
> Next, get the SingPass Face (SPFace) Verification Token from the `/face/verify/token/` endpoint, which is generated through the NRIC number provided on a form. 
> 
> This SPFace Token should be injected to your biometrics module – the `<sp-face>` HTML object, under the `token` attribute like so `<sp-face token="YOUR_TOKEN">`
> 
> You can customise the `<sp-face>` object by inserting `slots` and attributes.
> 
> Once the identity is verified, you need to call the API again to validate the results. This is to prevent Man-in-the-Middle attacks.

## FaceCompare Backend Service

**This backend service can be your authenticator, where all your requests will pass through.**

The frontend will call the RESTful APIs to transmit form data and images over, or to retrieve the OAuth 2.0 access tokens and Face Verification tokens.


FaceCompare functions used in this example SDK:

**getAuthToken()**

This function calls the SingPass API `/face/oauth/token/` and sends the following headers:

```
{
    "Authorization": "Basic " + auth,
    "Content-Type": "application/x-www-form-urlencoded"
}
```
as well as the following data:

```
{
    "grant_type": "client_credentials",
    "client_id": SECRET,
    "client_secret": CLIENT_SECRET
}

```

and returns the OAuth 2.0 Access Token, which will be needed for all SingPass API routes.

****

**getFaceVerifyToken()**

This function calls the SingPass API endpoint `/face/verify/token` for use in the verification module `<sp-face>` object injected into your HTML page.

From your OAuth 2.0 Access Token above, insert it in the headers of the following:

```
{
    "Authorization": "Bearer " + oauth_token,
    "Content-Type": "application/json",
}
```

with the following payload or data:

```
{
    "service_id": service_id,
    "user_id": user_id,
    "transaction_type": transaction_type
}

```
and returns the token that has to be injected in the frontend biometrics module.

****

**validateVerify()**

Once the user has their identity validated by the `<sp-face>` biometrics module, call this function to validate the result. This is to prevent man-in-the-middle attacks where cyber criminals might get ahold of the user's access token and use it to authenticate themselves.

You can only validate the token once, through the SingPass API endpoint `/face/verify/validate`. Following the first validation, it will return "Invalid token" on subsequent attempts to validate.

Just like the faceSDK() function you have to send the following headers:

```
{
    "Authorization": "Bearer " + oauth_token,
    "Content-Type": "application/json",
}
```

and send the following payload or data:

```
{
    "service_id": service_id,
    "user_id": user_id,
    "token": face_verify_token, // the SPFace token injected in the biometrics module
    "ip": ip, // YOUR FRONT-END IP ADDRESS
    "client": user_agent // YOUR FRONT-END USER AGENT OR BROWSER
}

```
This will return a boolean of whether the token has passed validation, the score of matching, reason for the result and the token.

## NuxtJS UI Frontend Service

In the Vue front-end, the `<sp-face>` biometrics module can be added to your HTML file like so:

```
<div class="content">
    <!-- SPFace Component -->
    <sp-face :token="token" v-if="validated" base_url="https://stg-bio-stream.singpass.gov.sg" logo="https://www.ndi-api.gov.sg/assets/img/ndi-api-logo.png">
        <!-- Custom slots go in here -->
    </sp-face>
</div>

```
**NOTE:** The `base_url` and `token` attribute is required. Inject the token from faceSDK() above into the `token` attribute, and the SingPass Face API to the `base_url`.

You could use the default functions, or customise the `<sp-face>` biometrics module like this:

```
<div slot="passed" class="m-1">
    <p class="has-text-grey-dark">Thank you. Your identity has been validated.<br></p>
    <!-- Redirect to the next action on successful validation -->
    <a class="button is-success" :href="nextURL">Continue</a>
    <!-- If you wanna retry, this button works too -->
    <a class="button is-info" href="/">Restart verification</a>
</div>

```

For Vue, to import the script, you could do it this way:

```
<script>
	export default {
		head () {
		    return {
		        ...
		        script: [
		            // Link to the staging environment of spface.js
		            { src: 'https://stg-bio-face.s3-ap-southeast-1.amazonaws.com/html5sdk/spface.js' },
		        ],
		        ...
		    }
		},
		...
	}
</script>
```

## Tying it all together

On page load, the frontend will call the frontend function `getAuthToken()`, calling the backend's own `getAuthToken()` endpoint, which will in turn, call the SingPass API endpoint `/face/oauth/token/`.

Once the OAuth token is returned, get the NRIC details from the user, and the frontend will call the `validateNRIC()` function, which calls the backend's `faceSDK()` function. This backend endpoint will call the SingPass API endpoint `/face/verify/token`, returning the biometrics module token if the NRIC is valid.

The user can then proceed to use the biometrics module, and on successful validation, the user will click on the "Continue" button. This will direct the user to the next page, which will send the token to the backend `validateVerify()` function, sending it through to the SingPass `/face/verify/validate` API endpoint.

You can choose what to do with the user flow using the data returned from the SingPass `/face/verify/validate` API.
