## How to setup HTTPS locally with create-react-app

Generally, we saw that the React app running locally on `http://localhost:3000`. But when we deploy and run the app on production we mostly host on HTTPS. Running HTTPS in development is helpful when you need to consume an API that is also serving requests via HTTPS.

Hello, Today we are going to discuss **How to serve a local React app via HTTPS.** In this article, we will be setting up HTTPS in development for our create-react-app with our SSL certificate.

 [Create-react-app (CRA)](https://reactjs.org/docs/create-a-new-react-app.html)  is a convenient and easy way to set up initial boilerplate when developing React App.  To start building React App using CRA, we generally use the following steps to start the setup and run the default CRA app.

```bash
npx create-react-app dummy-app
cd dummy-app
npm start
```

After running these steps our react app runs locally at `http://localhost:3000`. But we want to runs it at `https://localhost:3000`. So, the question is how do we run or test the app locally on https.

This is a very simple process for setup own SSL certificate and HTTPS in development. For this guide,  [brew](https://brew.sh/)  pre-installed required.

### Step 1: Adding HTTPS
In your `package.json`, update the start script.

```javascript
"scripts": {
    "start": "HTTPS=true react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
}
```

Now run `npm start` or `yarn start` and your browser screen show this-

![Privacy Error](https://cdn.hashnode.com/res/hashnode/image/upload/v1596199757184/zo_oZAjfQ.png)

### Step 2: Creating an SSL Certificate
At the previous stage, you're already set to go with https. But you don't have a valid certificate, so your connection is assumed to be insecure. So now we create our SSL certificate that works with the localhost. The easiest way to obtain a certificate is via [mkcert](https://github.com/FiloSottile/mkcert).

Run these commands in the terminal-
```bash
# Install mkcert tool
brew install mkcert

# Setup mkcert on your machine (creates a CA)
mkcert -install
```

![Setup mkcert on your machine](https://cdn.hashnode.com/res/hashnode/image/upload/v1596200142159/1W5O0tt0_.png)

After running the above commands, you'll have created a certificate authority on your machine which enables you to generate certificates for all of your future projects.

From the root of your `create-react-app` project, you should now run:
```bash
# Create .cert directory if it doesn't exist
mkdir -p .cert

# Generate the certificate (run from the root of this project)
mkcert -key-file ./.cert/key.pem -cert-file ./.cert/cert.pem "localhost"
```

![Generate the certificate](https://cdn.hashnode.com/res/hashnode/image/upload/v1596200390565/Qb1Q68do7.png)

We'll be storing our generated certificates in the `.cert` directory. These should not be committed to version control, so you should update your `.gitignore` to include the `.cert` directory.

### Step 3: Include SSL certificate in start script
Next, we need to update the start script again to include our newly created certificate:

```javascript
"scripts": {
    "start": "HTTPS=true SSL_CRT_FILE=./.cert/cert.pem SSL_KEY_FILE=./.cert/key.pem react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
}
```

When you run `npm start` or  `yarn start` again, you should now see that your connection is secure.


![How to setup HTTPS locally with create-react-app](https://cdn.hashnode.com/res/hashnode/image/upload/v1596200731237/BGSOnUvj-.png)


> **Note:** start script may have some changes for Windows, Linux, and Mac users.

Don't be a non-interactive person! Feel free to comment down your suggestions and questions, I am always open for your queries. 

Follow me on [Instagram](https://instagram.com/iamganeshagrawal) or  [Linkedin](https://www.linkedin.com/in/iamganeshagrawal/). Subscribe my weekly  [Newsletter](https://tinyletter.com/ganeshagrawal) , I assure its not going to spam your mailbox.