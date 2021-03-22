

# Amplifyapp

This project is based on [build react app amplify graphql tutorial](https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/).


## Bug #1
There were some bugs in the tutorial because the resources created underneath via cloudformation are not complete. On module 3 "Add Authentication" step "Set up CI/CD of the front end and backend", an error in the backend occurs:
```
JSONValidationError: File project: data should NOT have additional properties: 'graphqltransformer'
...
```
Solution: 
You need add a service role for Amplify console as github user **aam918** indicated [here](https://github.com/aws-amplify/amplify-console/issues/1345) and the complete instructions of how to do so is [here](https://docs.aws.amazon.com/amplify/latest/userguide/how-to-service-role-amplify-console.html).


## Bug #2 
Some necessary GraphQL functions have been renamed.
```
Failed to compile.

./src/App.js
Attempted import error: 'createNote' is not exported from './graphql/mutations' (imported as 'createNoteMutation').
```
The code provided in module 4 "Add API and Database" step "Write front-end code to interact with the API" is out of date. Variable names have changed. 
In file App.js, change line 4 and 5 to the following:
```
import { listTodos as listNotes } from './graphql/queries';
import { createTodo as createNoteMutation, deleteTodo as deleteNoteMutation } from './graphql/mutations';
```

They were the following before, which is refernece old variable names:
```
import { listNotes } from './graphql/queries';
import { createNote as createNoteMutation, deleteNote as deleteNoteMutation } from './graphql/mutations';
```

## Bug #3
Runtime error when logged in
```
Unhandled Rejection (TypeError): Cannot read property 'items' of undefined
```
The reason for this bug is that there are initially no notes but the first thing that loads in the page after login is to fetch all notes and display them.

To resolve add a simple check to see if anything is returned, change lines 18-21:
```
 18  async function fetchNotes() {
 19    const apiData = await API.graphql({ query: listNotes });
 20    setNotes(apiData.data.listNotes.items);
 21  }
```
to:
```
18  async function fetchNotes() {
19    const apiData = await API.graphql({ query: listNotes });
20    if (apiData.data.listNotes){
21      setNotes(apiData.data.listNotes.items);
22    }
23  }
```

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).


## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
