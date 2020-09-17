# Identiface Web Sample Application

base_url: `http://localhost:3000/sample-app/`

This is a sample application for SingPass Face Verification Service ("Identiface"). Consider using the [QuickStart Guide](http://developer.bio-api.singpass.gov.sg/guide/quickstart.html) for rapid prototyping without having to implement the backend.

> Note that UAT API keys and the X.509 Public Certificate are located in the Welcome Package for Developers. You will be issued one upon a successful link-up request.

----

Face Verify Sample: `https://www.identiface.live/sample-app/transactions` or `ui/pages/transactions`

> Face Verify contains an SDK (in HTML5, iOS, Android SDKs) to verify your user's face-biometrics features against the Government's biometrics data source. This SDK includes Presentation-Attack-Detection and liveness detection technologies to verify the user's identity authenticity.

Face Compare Sample: `ui/pages/employee-image`

> Face Compare is a direct API endpoint to match the user against the Government's biometrics data source. You could choose to upload static images of the user and our backend will compare the image against the SingPass Face database to return you a match result.

QuickStart backend API: `https://www.identiface.live/api`

> This backend is used to serve the frontend for QuickStart purposes and communicates directly with the SingPass API endpoints.

## Test Accounts

These dummy accounts will always either returns a `true` or `false` on **matching results**. Use either to design your reference journey flow for the linkup requests, or simple prototyping:

**G2957839M** returns `true`

**G2834561K** returns `false`

## Stack

**Frontend**: NuxtJS on port `3000`
**Backend**: Python-Flask on port `9000`

## Prerequisites

You need the Face backend service for this sample application, which is found [here](https://github.com/theodorayy/ndi-identiface-sample-public-backend).

For the frontend, you need `npm`. Also consider checking out the documentation for [NuxtJS](https://nuxtjs.org) if you require troubleshooting.

To set the connection between the frontend and backend, edit the value `baseURL` in `/ui/nuxt.config.js`:

```
axios: {
    // proxyHeaders: false
    // baseURL: "http://localhost:9000", // use this for dev environment
},
```

## Installing & Running

**On Development Environments**

Clone this repository and place these files where you want it to run.

Open up **Terminal** and `cd` into the `ui` folder that is inside your repository, and run the following command:

```
npm install
npm run dev
```

This is to start the **frontend** service.You can access the application through **http://localhost:3000/sample-app/**.

## Built With

* [NuxtJS](https://nuxtjs.org/guide) - The framework used for Server-Side Rendering (SSR)
* [Buefy](https://buefy.org) - Lightweight UI components based on [Bulma](https://bulma.io)

## License

Please abide by the SingPass [Terms of Use](https://www.singpass.gov.sg/singpass/common/termsofuse).

-----

_We appreciate any feedback you'd like to provide to our [NDI Biometrics Team](mailto:biometrics_support@ndi.gov.sg) email address. Thanks!_

---- 
