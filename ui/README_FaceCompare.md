# SingPass Face Compare Example

> ### TL;DR
> 
> First, to use the SDK, you need to get the OAuth 2.0 Access Token from the `/face/oauth/token/` endpoint using your Client ID & Secret.
> 
> Next, you can choose to upload static images of the user, or a simple webcam upload and send it to the `/face/compare` SingPass API endpoint.

## FaceCompare Backend Service

**This backend service can be your authenticator, where all your requests will pass through.**

The frontend will call the RESTful APIs to transmit form data and images over, or to retrieve the OAuth 2.0 access tokens and Face Verification tokens.


FaceCompare functions used in this example:

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

**faceCompare()**

This function sends the uploaded base64 encoded image of your user to the SingPass API endpoint `/face/compare`

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
    "image": image,
    "transaction_type": transaction_type //optional
}

```
and returns the matching score of the uploaded image compared to the SingPass database.

## Tying it all together

The frontend is the interaction point for the user to upload their image, or for you to capture their image quickly, and the backend sends the image to the API endpoint, which will return the matching score.

You can choose what to do with the user flow using the matching score data returned from the SingPass `/face/compare` API.
