## Deploy your server

* Create the Heroku app: `heroku create PROJECT_NAME`.
* connect your git repo to Heroku’s: `heroku git:remote -a PROJECT_NAME`
* When server is finalized, push your code to Heroku: `git push heroku master`.

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
  module.exports = {
    CLIENT_ORIGIN : process.env.CLIENT_ORIGIN || 'http://localhost:3000',
    PORT : process.env.PORT || 8080
  }
  ```
  * Double-check that your async actions still return your data.
  
### Get your client to Zeit

* Create a `now.json` file in root directory containing the following code:

```js
 {
 "version": 2,
 "name": "petful",
 "alias": "petful",
 "routes": [
   {
     "src": "^/static/(.*)",
     "dest": "/static/$1"
   },
   {
     "src": "^/favicon.ico$",
     "dest": "/favicon.ico"
   },
   {
     "src": "^/manifest.json$",
     "dest": "/manifest.json"
   },
   {
     "src": "^/site.webmanifest.json$",
     "dest": "/site.webmanifest.json"
   },
   {
     "src": ".*",
     "dest": "/index.html"
   },
   {
     "src": "^/favicon.ico$",
     "dest": "/favicon.ico"
   }
 ]
}


```
* ~Create a Heroku app for your client, using the [Create React App buildpack](https://github.com/mars/create-react-app-buildpack):~
  ```js
  heroku create --buildpack https://github.com/mars/create-react-app-buildpack.git
  ```

# TODO NOTES:
* _from this point, it needs complete re-write_
* _no env vars on Zeit client. Must be done during build process_
* _also refer them to:_ 
https://courses.thinkful.com/ei-react-v1/checkpoint/19  
_for specific details & steps_

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

