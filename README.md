# Identiface SDK Sample Application

This is a sample application for SingPass Face Verification Service ("Identiface"). Consider using the [QuickStart Guide] (https://www.identiface.live/docs) for rapid prototyping without having to implement the backend.

> Note that UAT API keys and the X.509 Public Certificate are located in the Welcome Package for Developers. You will be issued one upon a successful link-up request. 

-

Face Verify Sample: `https://www.identiface.live/html_sdk`

> Face Verify contains an SDK (in HTML5, iOS, Android SDKs) to verify your user's face-biometrics features against the Government's biometrics data source. This SDK includes Presentation-Attack-Detection and liveness detection technologies to verify the user's identity authenticity.

Face Compare Sample: `https://www.identiface.live/compare`

> Face Compare is a direct API endpoint to match the user against the Government's biometrics data source. You could choose to upload static images of the user and our backend will compare the image against the SingPass Face database to return you a match result.

Backend API: `https://www.identiface.live/api`

> This backend is used to serve the frontend and communicates directly with the SingPass API endpoints.

## Test Accounts

These dummy accounts will always either returns a `true` or `false` on **matching results**. Use either to design your reference journey flow for the linkup requests, or simple prototyping:

**G2957839M** returns `true`

**G2834561K** returns `false`

## Stack

**Frontend**: NuxtJS on port `3000`

**Backend**: Python-Flask on port `9000`

## Prerequisites

For the Face service, the requirements are:

* **pip3**
* **Python 3+**
* **Flask**
* **jwcrypto 0.7** – to verify JWS signature

For the frontend service, you need `npm`. Also consider checking out the documentation for [NuxtJS] (https://nuxtjs.org) if you require troubleshooting.

Next, to set the connection between the frontend and backend, edit the value `baseURL` in `/ui/nuxt.config.js`:

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

This is to start the **frontend** service.

Next, place the **run.sh** file that you have received from our guide to the `microsvc/face` folder, and run the following commands:

```
sh install.sh
sh run.sh
```
This will run the **backend** service.

You can access the application through **http://localhost:3000**.

## Built With

* [VueJS] (https://vuejs.org) - Javascript Framework for the front-end
* [NuxtJS](https://nuxtjs.org/guide) - The framework used for Server-Side Rendering (SSR)
* [Buefy] (https://buefy.org) - Lightweight UI components based on [Bulma] (https://bulma.io)
* [Docker](https://www.docker.com/) - Used to containerise different microservices
* [Flask](https://flask.palletsprojects.com/en/1.1.x/) - Python Web Server

## Contributors

* **[Emmanuel Rayendra] (https://theodorayy.github.io/)**, GovTech – *UI + backend Flask API, documentation*

## License

Please abide by the SingPass [Terms of Use] (https://www.singpass.gov.sg/singpass/common/termsofuse).

## Acknowledgments

* [linuxserver/letsencrypt] (https://hub.docker.com/r/linuxserver/letsencrypt/) – nginx container for easy SSL setup