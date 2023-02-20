
## AWS Amplify

AWS S3 hosting with Amplify: https://aws.amazon.com/amplify


AWS Amplify is a fully managed mobile application development service that provides a range of tools and libraries to accelerate and simplify the mobile application development process. It includes front-end development tools and libraries for popular frameworks and libraries such as React, React Native, Angular, Vue.js, back-end services including API services, data storage, authentication, push notifications, and a fully managed cloud services platform for building, deploying, and managing applications. It supports automated build and deployment processes and provides rich performance and security monitoring tools. Amplify also provides a range of analytics and monitoring tools to help developers monitor the performance and security of their applications and identify and resolve issues in a timely manner. Overall, AWS Amplify aims to provide developers with a comprehensive mobile application development platform to help them build and deploy applications more quickly and efficiently, thereby accelerating time-to-market for their products.


##### Install

```shell
npm install -g @aws-amplify/cli
```

configure
```shell
amplify configure
```

The `amplify configure` command is an important step in setting up and using the Amplify CLI to develop and deploy your Amplify projects on AWS.

The `amplify configure` command is used to configure the Amplify CLI with the credentials required to interact with AWS services.

When you run the amplify configure command, you will be prompted to enter your AWS access key ID and secret access key, as well as the region that you want to use for your AWS services. These credentials will be used by the Amplify CLI to interact with your AWS account and provision the necessary resources for your Amplify project.


##### Hosting a react app on AWS

1. Initialize app with Amplify by running the following command in your app's root directory:

    ```
    amplify init
    ```
    This command will prompt you to configure your Amplify project, such as the name of your app, the AWS region to use, and the type of app you are building.

2. Add hosting to your app by running the following command:

    ```
    amplify hosting add
    ```

    This command will prompt you to configure your hosting options, such as the type of hosting you want to use and the domain name you want to use.

3. Deploy your app to AWS by running the following command:
    ```
    amplify publish
    ```
    This command will build and deploy your app to AWS, and give you a URL where you can view your app.


##### Adding user auth
1. Add authentication to your project by running the following command:
   ```shell
    amplify add auth
   ```
   This command will prompt you to configure your authentication options, such as the type of authentication you want to use and the user attributes you want to collect.

2. Deploy your authentication resources by running the following command:
   ```shell
    amplify push
   ```
   This command will create the necessary AWS resources to support user authentication in your project.

3. Install the Amplify library and the AWS Amplify React library by running the following command:
   ```shell
    npm install aws-amplify aws-amplify-react
   ```

4. Initialize Amplify in your project by adding the following code to the entry point of your app (usually index.js or App.js):
   ```tsx
    import Amplify from 'aws-amplify';
    import awsconfig from './aws-exports';
    Amplify.configure(awsconfig);
   ```
   This code will configure your app with your Amplify project settings.

5. Create a sign-up and sign-in form using the withAuthenticator higher-order component from the AWS Amplify React library. Here's an example:
    ```
    import { withAuthenticator } from 'aws-amplify-react';

    function App() {
    return (
        <div>
        <h1>Welcome to my app</h1>
        </div>
    );
    }

    export default withAuthenticator(App);
    ```
    This code will create a sign-up and sign-in form that uses the Amplify authentication service.
6. Customize the sign-up and sign-in form by passing options to the withAuthenticator higher-order component. Here's an example:
   ```tsx
    export default withAuthenticator(App, {
        signUpConfig: {
            hiddenDefaults: ['phone_number'],
            signUpFields: [
                {
                    label: 'Email',
                    key: 'email',
                    required: true,
                    type: 'email',
                    displayOrder: 1,
                },
                {
                    label: 'Password',
                    key: 'password',
                    required: true,
                    type: 'password',
                    displayOrder: 2,
                },
            ],
        },
    });
   ```
   This code will customize the sign-up form to only include the email and password fields, and hide the phone number field.


##### AppSync

AWS AppSync is a fully managed service that makes it easy to develop GraphQL APIs by handling the heavy lifting of securely connecting to data sources like AWS DynamoDB, AWS Lambda, and other HTTP data sources. Here are some of the key concepts of AppSync:

1. Data sources: AppSync allows you to connect to a variety of data sources, including DynamoDB, Lambda, HTTP endpoints, and more. You can also create your own custom data sources by implementing a resolver.

2. Resolvers: A resolver is a function that maps a GraphQL field to a data source. Resolvers can be created for each field in your schema, and can include transformations, validation, and mapping logic.

3. Schema: Your GraphQL schema defines the structure of your API, including the types of objects, queries, and mutations that can be made. You can create and manage your schema using the AWS AppSync console or by defining it in a schema.graphql file.

4. Subscriptions: AppSync allows you to add real-time capabilities to your API by using subscriptions. Subscriptions allow clients to subscribe to changes in data and receive real-time updates over a WebSocket connection.

6. Authorization and authentication: AppSync provides built-in support for authorization and authentication. You can use Amazon Cognito to authenticate users, and AppSync integrates with AWS IAM to provide fine-grained access control for your API.

7. íVTL: Velocity Template Language (VTL) is a scripting language used in AppSync to define the logic for processing and transforming data. VTL allows you to perform operations like mapping, filtering, and formatting data, and is used in resolvers and pipeline functions.

These are some of the key concepts of AppSync. By understanding these concepts, you can create and manage powerful GraphQL APIs using AWS AppSync.


###### Creating a GraphQL API
- Add a GraphQL API: After you have initialized your Amplify project, you can add a GraphQL API to your project by running the `amplify add api` command. This command will launch a wizard that will guide you through the process of creating a new GraphQL API.
- Define your schema: Once you have added a GraphQL API to your project, you need to define your schema using the GraphQL Schema Definition Language (SDL). You can define your schema by editing the `schema.graphql` file in your project directory.
- Implement your resolvers: After you have defined your schema, you need to implement your resolvers, which are functions that map your GraphQL queries to data sources. You can implement your resolvers using AWS Lambda functions or any other data source that Amplify supports.
    ```js
    // Import the Amplify GraphQL libraries
        import { API, graphqlOperation } from 'aws-amplify';

        // Define the GraphQL query
        const listTodos = /* GraphQL */ `
        query ListTodos {
            listTodos {
            items {
                id
                name
                description
                completed
            }
            }
        }
        `;

        // Define the resolver function
        async function getTodos() {
        try {
            // Call the GraphQL API using Amplify
            const result = await API.graphql(graphqlOperation(listTodos));
            // Return the list of todos from the API response
            return result.data.listTodos.items;
        } catch (error) {
            // Handle any errors that occur
            console.error('Error fetching todos:', error);
            return [];
        }
        }

        // Export the resolver function
        export default getTodos;
    ```
- Deploy your API: Once you have implemented your resolvers, you can deploy your API to the cloud by running the `amplify push` command. This command will package and deploy your API to the AWS cloud, where it can be accessed by your clients.
- Connect to your API: After you have deployed your API, you can connect to it using a GraphQL client library, such as Apollo Client or Relay. You can use these libraries to query and mutate your data, and to subscribe to real-time updates using GraphQL subscriptions.
  1. Install the apollo-client and aws-appsync packages:
  ```
    npm install apollo-client aws-appsync
  ```
  2. Import the necessary libraries and configure your API:
  ```js
    import AWSAppSyncClient from 'aws-appsync';
    import { ApolloClient } from 'apollo-client';
    import { InMemoryCache } from 'apollo-cache-inmemory';
    import { createHttpLink } from 'apollo-link-http';

    const client = new AWSAppSyncClient({
        url: '<API_URL>',
        region: '<AWS_REGION>',
        auth: {
            type: 'API_KEY',
            apiKey: '<API_KEY>',
        },
    });

    const httpLink = createHttpLink({
     uri: '<API_URL>',
    });

    const apolloClient = new ApolloClient({
        link: httpLink,
        cache: new InMemoryCache(),
    });
  ```
3. Define your queries and mutations:
   ```
    import gql from 'graphql-tag';

    const listTodos = gql`
    query ListTodos {
        listTodos {
        items {
            id
            name
            description
            completed
        }
        }
    }
    `;

    const createTodo = gql`
    mutation CreateTodo($name: String!, $description: String!, $completed: Boolean!) {
        createTodo(input: { name: $name, description: $description, completed: $completed }) {
        id
        name
        description
        completed
        }
    }
    `;
   ```
   In this example, we define two GraphQL operations: a listTodos query that fetches a list of todos, and a createTodo mutation that creates a new todo.
4. Call your queries and mutations:
   ```ts
    apolloClient.query({ query: listTodos }).then((result) => {
    console.log('Todos:', result.data.listTodos.items);
    });

    apolloClient
    .mutate({ mutation: createTodo, variables: { name: 'New Todo', description: 'Description', completed: false } })
    .then((result) => {
        console.log('Created Todo:', result.data.createTodo);
    });
   ```
   