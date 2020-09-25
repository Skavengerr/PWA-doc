Documentation for Magento PWA Studio packages is located at [https://pwastudio.io](https://pwastudio.io).


## Step 1. Create magento PWA app

MacOs / Linux setup `yarn create @magento/pwa` and create the application according to the installation steps

Windows setup `git clone https://{your bitbucketname}@bitbucket.org/qlicksdev/magento-studio.git`

## Step 2. Install Dependencies

`yarn install`

## Step 3. Use the create-custom-origin sub-command from the buildpack CLI to create a custom hostname and SSL cert:

`yarn buildpack create-custom-origin ./`

## Step 4. Start the server

`yarn run watch`

Open a new tab with a link that shows you in the console

![image](https://user-images.githubusercontent.com/41162650/94237057-edab2080-ff16-11ea-9c3b-21c25314d06a.png)

### Step 5. Linting and testing

The Venia concept project also contains scripts for formatting (`yarn run prettier`), style analysis (`yarn run lint`), and running unit tests (`yarn test`).

Use these scripts to keep your codebase well-formatted and test functionality.

## Step 6. Storybook testing

To run storybook use command:

`yarn storybook`

### storybook structure

![image](https://user-images.githubusercontent.com/41162650/93874211-b4d33780-fcdb-11ea-8506-cb8860b808e8.png)
![image](https://user-images.githubusercontent.com/41162650/93874277-cae0f800-fcdb-11ea-9c41-e6cbca2fc0a4.png)

To run lint

## Step 7. Work With GraphQl

#### Request country data

In the code, the component imports and destructures the GraphQL query from the `countries.gql.js` file and makes a request using `useQuery()` from the `@apollo/client` library.

```jsx
import React from 'react';
import {gql} from '@apollo/client' // used for sending graphql requests
import { useQuery } from '@apollo/client'; // used for parsing response from backEnd

const GET_COUNTRIES_QUERY = gql`
	query getAllCountries {
		countries {
			available_regions {
				code
				id
				name
			}
			id
		}
	}
`

const Countries = () => {
    const { queries } = GET_COUNTRIES_QUERY;
    const { getCountriesQuery } = queries;

    // Fetch the data using apollo react hooks
    const { data, error, loading } = useQuery(getCountriesQuery);

    // Loading and error states can detected using values returned from
    // the useQuery hook
    if (loading) {
        return <span>Loading...</span>;
    }

    if (error) {
        // This is only meant to show WHERE you can handleGraphQL errors
        return <span>Error!</span>;
    }

    const { countries } = data;

return (
        <ul>
            {countries.map(country => {
                <li key={country.id}>
                    {country.name}
                </li>
            })}
        </ul>
    )
};

export default Countries;
```

## Step 8. Associate components to routes

Use the steps in the [Add a static route][] tutorial to add the following entries to the Routes component:

`./local-intercept.js`

```js
module.exports = targets => {
  targets.of("@magento/venia-ui").routes.tap(routes => {
    routes.push({
      name: "MyGreetingRoute", // name of component
      pattern: "/greeting", // url for this components
      path: require.resolve("../components/GreetingPage/greetingPage.js") // path to component
    });
    return routes;
  });
};
```

