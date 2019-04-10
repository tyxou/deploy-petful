## Deploy your server

* Create the Heroku app: `heroku create PROJECT_NAME`.
* Push your code to Heroku: `git push heroku master`.

## Deploy your client

### Do some server-side preparation

Make sure your client is configured to [access data from a variable server location](https://courses.thinkful.com/react-001v3/assignment/1.2.1). Be sure to read and follow these instructions carefully!
  * Make sure you've added the following to `server/index.js`:
  ```js
    app.use(cors({
      origin: CLIENT_ORIGIN
    });
  ```
  * In `server/src/config.js`, make sure you've exported the following:
  ```js
  CLIENT_ORIGIN = process.env.CLIENT_ORIGIN || 'http://localhost:3000'
  ```
  * Double-check that your async actions still return your data.
  
### Get your client to Heroku

* Create a `static.json` file in root directory containing the following code:
```js
  {
    "root": "build/",
    "routes": {
      "/**": "index.html"
    }
  }
```
* Create a Heroku app for your client, using the [Create React App buildpack](https://github.com/mars/create-react-app-buildpack):
  ```js
  heroku create --buildpack https://github.com/mars/create-react-app-buildpack.git
  ```

* Set your API's base URL as an environment variable in Heroku: 
  ```js
  heroku config:set REACT_APP_API_BASE_URL=https://MY-SERVER-URL.herokuapp.com/api
  ```

* Push your code to Heroku: `git push heroku master`.

## Configure the server to accept requests from the client

* Add a `CLIENT_ORIGIN` environment variable to your Heroku app pointing to your deployed client:
  ```js
  heroku config:set CLIENT_ORIGIN=https://MY-CLIENT_URL.herokuapp.com
  ```
  * Make sure there is _no_ trailing slash!
* Run `heroku restart` to ensure that the app runs with knowledge of the new client origin.

