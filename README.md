# PWA-doc
The docs for pwa application with magento

### Step 1. Clone the PWA Studio repository into your development environment.

`git clone https://github.com/magento/pwa-studio.git`

### Step 2. Install PWA Studio dependencies

`yarn install`

### Step 3. Generate SSL certificate

PWA features require an HTTPS secure domain.

`
yarn buildpack create-custom-origin packages/venia-concept
`

### Step 4. Create the `.env` file

Use the `create-env-file` subcommand for the `buildpack` CLI tool to create a `.env` file for Venia.
The subcommand generates a `packages/venia-concept/.env` file where you can set the value of `MAGENTO_BACKEND_URL` to the URL of a running Magento instance.

You can create the `.env` file and set the `MAGENTO_BACKEND_URL` value at the same time using a command similar to the following:

`
MAGENTO_BACKEND_URL="https://master-7rqtwti-mfwmkrjfqvbjk.us-4.magentosite.cloud/" \
CHECKOUT_BRAINTREE_TOKEN="sandbox_8yrzsvtm_s2bg8fs563crhqzk" \
yarn buildpack create-env-file packages/venia-concept
`

### Step 5. Start the server

Before you run the server, generate build artifacts for Venia using the following command in the **project root directory**:

`yarn run build`

#### Run the server

Use any of the following commands from the **project root directory** to start the server:

`yarn run watch:venia`

: Starts the Venia storefront development environment.

`yarn run watch:all`

: Runs the full PWA Studio developer experience, which include Venia hot-reloading and concurrent Buildpack/Peregrine rebuilds.

### Storybook testing

To run storybook use command: 

`yarn storybook:venia`

#### storybook structure

![image](https://user-images.githubusercontent.com/41162650/93874211-b4d33780-fcdb-11ea-8506-cb8860b808e8.png)
![image](https://user-images.githubusercontent.com/41162650/93874277-cae0f800-fcdb-11ea-9c41-e6cbca2fc0a4.png)


## 5.Routing
