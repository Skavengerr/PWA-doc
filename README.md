Documentation for Magento PWA Studio packages is located at [https://pwastudio.io](https://pwastudio.io).

# MacOs / Linux setup

## Step 1. Clone project into your system

`git clone https://{your bitbucketname}@bitbucket.org/qlicksdev/magento-studio.git`

## Step 2. Install Dependencies

`yarn install`

## Step 3. Use the create-custom-origin sub-command from the buildpack CLI to create a custom hostname and SSL cert:

-   copy `.env.example` to `.env` and change `MAGENTO_BACKEND_URL` if needed
-   `yarn buildpack create-custom-origin ./`

## Step 4. Start the server

`yarn run watch`

Open a new tab with a link that shows you in the console

![image](https://user-images.githubusercontent.com/41162650/94237057-edab2080-ff16-11ea-9c3b-21c25314d06a.png)

# Windows setup

## Step 1. Install Ubuntu and Create symbolic links

Install VirtualBox and Ubuntu, create shared folders.

Create symbolic links within folders shared with the windows host system `VBoxManage.exe setextradata VM_NAME VBoxInternal2/SharedFoldersEnableSymlinksCreate/SHARED_NAME 1`

Open Local Security Policy, Navigate to: Local Policies -> User Rights Assignment, Open "Create Symbolic Links", Add your username (or a group you are assigned to), Restart PC

## Step 2. Install Dependencies

Install Nodejs and yarnpkg to your Ubuntu machine:

`sudo apt install nodejs`

`sudo apt-get install yarnpkg`

Clone magento-pwa-studio in your shared folder

`git clone https://{your bitbucket name}@bitbucket.org/qlicksdev/magento-studio.git`

Go into folder with your magento-pwa application `cd magento-studio`

Install Dependencies `yarnpkg install`

## Step 3. Env

-   copy `.env.example` to `.env` and change `MAGENTO_BACKEND_URL` if needed

## Step 4. Start the server

`yarnpkg watch`

Open a new tab with a link that shows you in the console

![image](https://user-images.githubusercontent.com/41162650/94237057-edab2080-ff16-11ea-9c3b-21c25314d06a.png)

## Linting and testing

This project also contains scripts for formatting (`yarn run prettier`), style analysis (`yarn run lint`), and running unit tests (`yarn test`).

Use these scripts to keep your codebase well-formatted and test functionality.

## Storybook testing

To run storybook use command:

`yarn storybook`

### storybook structure

![image](https://user-images.githubusercontent.com/41162650/94258107-04f90680-ff35-11ea-92ac-888f7650fa34.png)![image](https://user-images.githubusercontent.com/41162650/94258039-ed218280-ff34-11ea-9148-18254f27c77d.png)

## Associate components to routes

Use the steps in the [a tutorial](https://magento.github.io/pwa-studio/tutorials/pwa-studio-fundamentals/add-a-static-route/) to add the following entries to the Routes component:

`./local-intercept.js`

```js
function localIntercept(targets) {
	targets.of('@magento/venia-ui').routes.tap(routes => {
		routes.push({
			name: 'MyRoute', // name of component
			pattern: '/component', // url for this components
			path: require.resolve('./src/app/components/Component/component.js'), // path to component
		});
		return routes;
	});
}

module.exports = localIntercep;
```

Add the path to your intercept file in the pwa-studio.targets.intercept section of your project’s package.json file. This registers ./local-intercept.js as an intercept file during the build process.

```js
  "pwa-studio": {
    "targets": {
      "intercept": "./local-intercept.js"
    }
  },
```

## Add your components

At "main" component you can add your components

![image](https://user-images.githubusercontent.com/41162650/94538094-40087c00-024c-11eb-9ad6-e1918a7d604b.png)

## Work With GraphQl

#### Request country data

In the code, the component imports and destructures the GraphQL query and makes a request using `useQuery()` from the `@apollo/client` library.

```jsx
import React from 'react';
import gql from 'graphql-tag'; // A template literal tag for wrapping GraphQL strings
import { useQuery } from '@apollo/react-hooks'; // It's a Hook that fetches a GraphQL query

const GET_COUNTRIES_QUERY = gql`
	query GetCountries {
		countries {
			id
			full_name_english
		}
	}
`;

const Countries = () => {
	// Fetch the data using apollo react hooks
	const { data, error, loading } = useQuery(GET_COUNTRIES_QUERY);

	// Loading and error states can detected using values returned from the useQuery hook
	if (loading) {
		return <span>Loading...</span>;
	}

	if (error) {
		// This is only meant to show WHERE you can handleGraphQL errors
		return <span>Error!</span>;
	}

	if (!data) return null;

	const { countries } = data;

	return (
		<ul>
			{countries.map(country => (
				<li key={country.id}>{country.full_name_english}</li>
			))}
		</ul>
	);
};

export default Countries;
```
