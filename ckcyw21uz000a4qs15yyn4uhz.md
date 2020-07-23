## How to use environment variables in Node.js application

Deploying an application requires developers to put thought and consideration into how it is configured. Many apps are deployed in a development environment before being deployed to the production environment. We need to ensure each environment is configured correctly, it could be disastrous if our production application was using our development database, for example.

Environment variables are helpful in software development. Many programs and applications set or get environment variables.

Environment variables allow us to manage the configuration of our applications separate from our codebase. Separating configurations make it easier for our application to be deployed in different environments.

## What are environment variables?
Environment variables are variables external to our application that reside in the OS or the container of the app is running in. An environment variable is simply a name mapped to a value.

By convention, the name is capitalized e.g. `JWT_REFRESH_SALT=a3889318b21d7b92`. The values are strings.

If you open the terminal or command line application in Linux, Mac OS, or Windows and enter `set`, you will see a list of all environment variables for your user.


> ### Did you know?
When we install any software (like Java SDK, Language compiler, or databases, ...). In the case of many software, we have to set a `PATH` variable for that. That PATH variable also a part of the environment variable of your operating system.

## Why do we need environment variables?
Environment variables are excellent for decoupling application configurations. Typically, our applications require many variables to be set in order for them to work. By relying on external configurations, your app can easily be deployed in different environments. These changes are independent of code changes, so they do not require your application to be rebuilt to change.
- **Security:** Some things like API keys should not put in plain text in the code and thereby directly visible.
- **Flexibility:** you can reduce conditional statements e.g. *“If production server then do X else if test server then do Y else if localhost then do Z …”.*
- **Adoption:** Popular services like GitLab or Heroku support the usage of environment variables.

Data that changes depending on the environment your app is running on should be set as environment variables. Some common examples are:
- HTTP Port and Address
- Database, cache, and other storage connection information
- Location of static files/folders
- Endpoints of external services
 - For example, in a development environment, your app will point to a test API URL, whereas in a production environment your app will point to the live API URL.

> Sensitive data like API keys should not be in the source code or known to persons who do not need access to those external services.

## How does a .env file look like?
A .env file is more or less a plain text document. It should be placed in the root of your project. You specify key-value pairs and you can write single-line comments using #.

Example of .env file:
```env
# JWT
AUTH_TOKEN_SALT=a7f1681c86cee718
REFRESH_TOKEN_SALT=df4f2ffea4ddf7ef

# SERVER CONFIG
PORT=8080

# DATABASE
DB_HOSTNAME=mysql.example.com
DB_HOSTPORT=3606
DB_USERNAME=user
DB_PASSWORD=demo123
DB_DBNAME=testDb
```

## How to use custom environment variables in Node.js

1. Create a **.env** file. The file should be placed in the root of your project.
2. Install the `dotenv` npm module using:
```sh
npm install dotenv --save
# OR
yarn add dotenv
```
3. Require `dotenv` as early as possible (e.g. in app.js):
```javascript
require('dotenv').config({path: __dirname + '/.env'})
```
4. Wherever you need to use environment variables (e.g. in GitLab, in Jenkins, in Heroku, …) you need to add your environment variables. The way depends on the platform but it is usually easy to do.

## How to access environment variables in Node.js
Consider a hello world Node.js application with environment variables for the host and port the app runs on.

Create a new file called app.js in a workspace of your choice and add the following:
```javascript
const http = require('http');

// Load custom environment variables
require('dotenv').config();

// Read the host address and the port from the environment
const hostname = process.env.HOST;
const port = process.env.PORT;

// Return JSON regardless of HTTP method or route our web app is reached by
const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'application/json');
    res.end(`{"message": "Hello World"}`);
});

// Start a TCP server listening for connections on the given port and host
server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```
Also, create a new file called `.env` in the root of your app.js file and add the following:
```env
# Server Port & Host
PORT=4000
HOST=localhost
```

Node.js provides a global variable `process.env`, an object that contains all environment variables available to the user running the application. It is expecting the hostname and the port that the app will run on to be defined by the environment.

You can run this application by entering this command in the terminal, `node app.js`, granted you have Node.js installed. You will notice the following message on your console:
```bash
Server running at http://localhost:4000/
```

> **Note:** Even if dotenv is used in the application, it is completely optional. If no .env file is found, the library fails silently. You can continue to use environment variables defined outside the file.

## Production Usage
Storing your environment variables in a file comes with one golden rule: never commit it to the source code repository. You do not want outsiders to gain access to secrets, like API keys. If you are using `dotenv` to help manage your environment variables, be sure to include the `.env` file in your `.gitignore` or the appropriate blacklist for your version control tool.

If you cannot commit the `.env` file, then there needs to be some way for a developer to know what environment variables are required to run the software. It is common for developers to list the environment variables necessary to run the program in a **README** or similar internal documentation.

Some developers create and maintain a `.sample-env` file in the source code repository. This sample file would list all the environment variables used by the app, for example:
```env
# Server Port & Host
PORT=
HOST=
```
A developer would then clone the repository, copy the `.sample-env` file into a new `.env` file and fill in the values.

If your app is running on a docker container or a Platform-as-a-Service provider like Heroku or Openshift, then you will be able to configure environment variables without having to use the `.env` file.

**Remember**, it fails silently so it would not affect the running of the app if the file is missing.

## Conclusion
Thanks for reading this article. As you can see, it is easy to use and work with environment variables in Node.js. Like Node.js other languages also supports environment variable and have their respective modules for handling them. How are you using environment variables? Let me know in the comments.