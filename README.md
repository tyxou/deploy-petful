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

* Create a `now.json` file in the *public* directory of your client, containing the following code:

```js
 {
 "version": 2,
 "name": "CLIENT_PROJECT_NAME",
 "alias": "CLIENT_PROJECT_NAME",
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
* Before we can deploy to Zeit, we need to build the client files. Make sure to have a *_.env_* file with any global variables needed, like the API url. These env vars must start with:
`REACT_APP_`, or else, `react-scripts` will ignore them! For instance, in client’s .env file we might have:
```
REACT_APP_API_BASE=https://SERVER-PROJECT-NAME.herokuapp.com/api
```
and the _config.js_ file (client) would then read something like:
```js
export default {
  REACT_APP_API_BASE: process.env.REACT_APP_API_BASE || “http://localhost:8080/api”
}
```
Afterwards we can use `react-scripts` to build our client files so we can then deploy them to Zeit (making sure the _now.json_ file created earlier is inside the *public* folder):
```
  npm run build
```
or
```
  yarn run build
```
When the build process is done, we can use the created folder (named _build_) to deploy our static files to Zeit:
```
  now --name CLIENT_PROJECT_NAME ./build
```
Substitute the “CLIENT_PROJECT_NAME” with the name used in the `name` & `alias` in _now.json_. This will create a repo for us on Zeit with the specified name, containing the static files built by `react-scripts` inside the _build_ folder.


## Configure the server on Heroku to accept requests from the client

* Add a `CLIENT_ORIGIN` environment variable to your Heroku app pointing to your deployed client:
  ```js
  heroku config:set CLIENT_ORIGIN=https://CLIENT_PROJECT_NAME.now.sh
  ```
  * Make sure there is _no_ trailing slash!
  * Run `heroku restart` to ensure that the app runs with knowledge of the new client origin.

For further details on client deployment go to this checkpoint:
https://courses.thinkful.com/ei-react-v1/checkpoint/19  
