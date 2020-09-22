# PWA-doc
The docs for pwa application with magento

## Step 1. Clone the PWA Studio repository into your development environment.

`git clone https://github.com/magento/pwa-studio.git`

## Step 2. Install PWA Studio dependencies

`yarn install`

## Step 3. Generate SSL certificate

PWA features require an HTTPS secure domain.

`
yarn buildpack create-custom-origin packages/venia-concept
`

## Step 4. Create the `.env` file

Use the `create-env-file` subcommand for the `buildpack` CLI tool to create a `.env` file for Venia.
The subcommand generates a `packages/venia-concept/.env` file where you can set the value of `MAGENTO_BACKEND_URL` to the URL of a running Magento instance.

You can create the `.env` file and set the `MAGENTO_BACKEND_URL` value at the same time using a command similar to the following:

`
MAGENTO_BACKEND_URL="https://master-7rqtwti-mfwmkrjfqvbjk.us-4.magentosite.cloud/" \
CHECKOUT_BRAINTREE_TOKEN="sandbox_8yrzsvtm_s2bg8fs563crhqzk" \
yarn buildpack create-env-file packages/venia-concept
`

## Step 5. Start the server

Before you run the server, generate build artifacts for Venia using the following command in the **project root directory**:

`yarn run build`

#### Run the server

Use any of the following commands from the **project root directory** to start the server:

`yarn run watch:venia`

: Starts the Venia storefront development environment.

`yarn run watch:all`

: Runs the full PWA Studio developer experience, which include Venia hot-reloading and concurrent Buildpack/Peregrine rebuilds.



## Step 6. Linting and testing

The Venia concept project also contains scripts for formatting (`yarn run prettier`), style analysis (`yarn run lint`), and running unit tests (`yarn test`).

Use these scripts to keep your codebase well-formatted and test functionality.

#### Storybook testing

To run storybook use command: 

`yarn storybook:venia`

#### storybook structure

![image](https://user-images.githubusercontent.com/41162650/93874211-b4d33780-fcdb-11ea-8506-cb8860b808e8.png)
![image](https://user-images.githubusercontent.com/41162650/93874277-cae0f800-fcdb-11ea-9c41-e6cbca2fc0a4.png)



## Step 7.Work With GraphQl

#### Request country data

Create a `country.gql.js` file under `src/components/Country` with the following content:

```js
import { gql } from '@apollo/client';

const GET_COUNTRY_QUERY = gql`
    query GetCountry($id: String) {
        country(id: $id) {
            id
            full_name_english
            available_regions {
                id
                name
            }
        }
    }
`;

export default {
    queries: {
        getCountryQuery: GET_COUNTRY_QUERY
    },
    mutations: {}
};
```

This file defines a component that creates a list of countries where the backend Magento store does business.
Each country listed is a link that points to a page for that country.

In the code, the component imports and destructures the GraphQL query from the `countries.gql.js` file and makes a request using `useQuery()` from the `@apollo/client` library.

Next, create an `index.js` file under `src/components/Countries`.
Add the following content to export the Countries component from the directory.

Under `src/components/Country`, create a `country.js` file with the following content:

```jsx
import React, { useEffect } from 'react';
import { useQuery } from '@apollo/client';
import { useParams } from '@magento/venia-ui/lib/drivers';

import countryOperations from './country.gql';

import Regions from './regions';

const Country = () => {
    // Scroll to the top on page load
    useEffect(() => {
        window.scrollTo(0, 0);
    }, []);

    const { id } = useParams();

    const { queries } = countryOperations;
    const { getCountryQuery } = queries;

    // Fetch the data using apollo react hooks
    const { data, error, loading } = useQuery(getCountryQuery, {
        variables: { id: id }
    });

    if (loading) {
        return <span>Loading...</span>;
    }

    if (error) {
        // NOTE: This is only meant to show WHERE you can handle
        // GraphQL errors. Not HOW you should handle it.
        return <span>Error!</span>;
    }

    const { country } = data;

    const { full_name_english: name, available_regions: regions } = country;

    return (
        <div>
            <h2>{name}</h2>
            <Regions regions={regions} />
        </div>
    );
};

export default Country;
```

## Step 8. Associate components to routes

Use the steps in the [Add a static route][] tutorial to add the following entries to the Routes component:

/packages/venia-ui/lib/components/Routes

```diff
 <Suspense fallback={fullPageLoadingIndicator}>
     <Switch>
+        <Route exact path="/countries">
+            <Countries />
+        </Route>
+        <Route path="/country/:id?">
+            <Country />
+        </Route>
         <Route exact path="/search.html">
             <Search />
         </Route>
         <Route exact path="/create-account">
             <CreateAccountPage />
         </Route>
         <Route>
             <MagentoRoute />
         </Route>
     </Switch>
 </Suspense>
```

This update associates the Countries and Country components with specific routes.
The pattern used for the country route lets the `useParams()` hook get the country ID from the URL.


-----------------------------------------------------------------------------------------------------


## Custom React hooks

Many of the functions provided by Peregrine are [custom React hooks][].
This lets them maintain an internal state without relying on an external library, such as Redux.

Peregrine hooks are designed to be flexible, and non-opinionated about UI.
They contain code for providing data or behavior logic and do not render content themselves.
Rendering content is left up to UI components.

Separating logic and presentation code gives developers more flexibility on how to use PWA Studio components with their own custom code.
A developer may choose to use a Venia feature that uses certain Peregrine hooks with minor visual modifications, or
they can use those same Peregrine hooks to develop their own feature with a different UI.

/packages/peregine/lib/hooks

![image](https://user-images.githubusercontent.com/41162650/93887678-ad1d8e00-fcef-11ea-838b-d2ad00a3b3b7.png)

