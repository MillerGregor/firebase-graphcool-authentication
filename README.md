# firebase-authentication

Create Firebase users and sign in with Schema Extensions and Graphcool Functions ⚡️

> Note: Schema Extensions are currently only available in the Beta Program.

## Authentication flow in the example app

1. The user clicks a `Authenticate with ____` button
1. The selected provider requests login and permission
1. 
1. The app receives a Firebase Access Token
1. Your app calls the Graphcool mutation `authenticateFacebookUser(facebookToken: String!)`
1. If no user exists yet that corresponds to the passed `facebookToken`, a new `User` node will be created
1. In any case, the `authenticateFacebookUser(facebookToken: String!)` mutation returns a valid token for the user
1. Your app stores the token and uses it in its `Authorization` header for all further requests to Graphcool

## Project Setup


### Graphcool Setup

- Create a new Schema Extension Function and paste the schema from `schema-extension.graphql`.
- Create a new Permanent Access Token (PAT) in project settings.
- Remove all Create permissions for the `User` type. The function uses a Permanent Access Token to create users via the API so the permissions are not needed.


### Firebase Setup

Create a Firebase project:
- <https://console.firebase.google.com/u/1/>

Copy your Firebase project's web sdk api keys:
- <https://firebase.google.com/docs/web/setup>

Create a Firebase Service Account :
- <https://console.cloud.google.com/projectselector/iam-admin/serviceaccounts>
- Download the provided private key

### Serverless Setup

Install the serverless CLI
```
yarn global add serverless
```
Edit the env-**_stage_**.yml file
- Insert the Graphcool PAT and ProjectId
- Insert the Firebase Service Account private key file's contents










Run this mutation to authenticate a user:

```graphql
mutation {
  # replace __FIREBASE_TOKEN__!
  authenticateFirebaseUser(firebaseIdToken: "__FIREBASE_TOKEN__") {
    token
  }
}
```

You should see that a new user has been created. The returned token can be used to authenticate requests to your Graphcool API as that user. Note that running the mutation again with the same Firebase token will not add a new user.