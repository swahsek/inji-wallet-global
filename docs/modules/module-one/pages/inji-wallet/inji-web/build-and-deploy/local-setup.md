# Local setup

This document aims to assist users in setting up Inji Web on their local environment, offering step-by-step instructions to replicate the platform's functionality on their machine for development or testing purposes.

### Repositories:

Clone the repositories locally to bring up your own setup. Repository information can be fetched from [**here**](https://github.com/mosip/inji-web.git).

**Pre-requisite:**

In order to run Inji Web locally, Node 18 is required. Please follow the below steps to install node.

Node 18 can be installed using [nvm](https://github.com/nvm-sh/nvm). Run the following commands to install node:

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
$ nvm install 18
```

#### Folder Structure:

* **helm:** folder contains helm charts required to deploy on K8S
* **inji-web:** contains the source code and Dockerfile
* **docker-compose:** contains docker-compose.yml, environment files, and service configurations.
* [README link](https://github.com/mosip/inji-web/blob/master/README.md)

***

## Run Inji Web locally using Docker Compose:

This setup uses Docker Compose to run **DataShare Service (Service to store VC)**, **Mimoto Service** (Backend for Frontend) and **Inji Web** (Frontend) together locally, providing a simple and easy-to-manage environment.

### Steps:

1. **Navigate to the `inji-web` folder and build the Inji Web image locally.**

```bash
cd ./inji-web
docker build -t inji-web:local .
```

2. **Prepare the environment:**
   * In the `docker-compose` folder, there are configuration files and certificates for integrating Mimoto as the BFF (Backend for Frontend) for the web and mobile services.
   * **Create a `certs` folder** in the `docker-compose` directory and add the necessary OIDC client certificate (as mentioned below).
   * **Update the configuration files:**
     * `mimoto-issuers-config.json` (Add Id providers as issuers)
     * `mimoto-trusted-verifiers.json` (Add verifiers clientId and redirect URI for online sharing)
     * Update **Esignet host references** in the `mimoto-default.properties`, `mimoto-issuers-config.json` files.
     * Set the `oidc_p12_password` environment variable value as per the documentation.
   * **Create OIDC client** and generate `oidckeystore.p12`. You can follow the setup guide for this process [here](https://docs.mosip.io/inji/inji-mobile-wallet/customization-overview/credential_providers).
   * **Update `mimoto-issuers-config.json`** with the client ID and client alias as per your OIDC onboarding.
3. **Start the Docker Compose environment:**

```bash
cd docker-compose
docker-compose up -d
```

4. **Stop the Docker Compose environment:**

```bash
docker-compose down
```

***

## Run Inji Web locally (Non-Docker Compose Setup):

If you'd prefer to run Inji Web without Docker Compose, you can manually set up and run the app on your local machine.

1. **Navigate to the `inji-web` folder and install dependencies:**

```bash
cd ./inji-web
npm install
```

2. **Start the Inji Web application:**

```bash
npm start
```

3.  **Troubleshooting CORS issues:**

    If you encounter CORS errors when running the web app locally, you will need to set up a proxy server and configure it to point to `https://api.collab.mosip.net` for Mimoto's API.

    * Follow this [guide](https://jakemccambley.medium.com/fixing-cors-errors-when-working-with-3rd-party-apis-a69dc5474804) to set up the proxy.
    * Once the proxy server is running, update the `Inji Web` app to use the proxy by modifying the `window.location.origin` in the `src/utils/api.ts` file.
4. **Run tests:**

```bash
npm test
```

***

### Summary:

* **Docker Compose Setup**: This is the recommended setup for running Inji Web along with Mimoto locally.
* **Standalone Setup**: If you prefer, you can run Inji Web as a standalone Node.js application.
