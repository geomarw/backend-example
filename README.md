# Backend example

This project was created for teaching purposes. It will be implemented from zero, step by step. The steps are stored in the branches.

## Step 1 - Create structure

### branch: [step-01-structure](https://github.com/geomarb/backend-example/tree/step-01-structure)

- creating the basic folder structure and files via terminal/console

## Project Structure Tree

```console
 / (root of the project)
|
+-- src/
|  |
|  +-- config/
|  |  |
|  |  +-- db.js
|  |
|  +-- controllers/
|  |  |
|  |  +-- auth.controller.js
|  |  |
|  |  +-- user.controller.js
|  |
|  +-- middlewares/
|  |  |
|  |  +-- auth.middleware.js
|  |  |
|  |  +-- error-handler.middleware.js
|  |
|  +-- models/
|  |  |
|  |  +-- user.model.js
|  |
|  +-- routes/
|  |  |
|  |  +-- index.js
|  |  |
|  |  +-- auth.routes.js
|  |  |
|  |  +-- user.routes.js
|  |
|  +-- app.js
|  |
|  +-- index.js
|
+-- .env
```

### Project Structure Tree creation via console/terminal

- clone in your local machine the repository you created in your GitHub account
- go into the cloned folder
- create all folders from above structure with `mkdir -p` command
- create all files listed about in the correct folders with the command `touch`
- create a new branch and checkout it in git
- stage all changes
- commit all changes
- push the committed changes to the remote repository in GitHub

```console
git clone git@github.com:geomarb/backend-example.git
cd backend-example
mkdir -p src/config src/controllers src/middlewares src/models src/routes
touch .env
touch src/index.js src/app.js
touch src/controllers/user.controller.js src/controllers/auth.controller.js
touch src/middlewares/auth.middleware.js src/middlewares/error-handler.middleware.js
touch src/models/user.model.js
touch src/routes/index.js src/routes/user.routes.js src/routes/auth.routes.js
git checkout -b step-01-structure
git add .
git commit -m "create structure"
git push --set-upstream origin step-01-structure
```

## Step 2 - Add NPM packages

### branch: [step-02-adding-npm-packages](https://github.com/geomarb/backend-example/tree/step-02-adding-npm-packages)

- create a new branch in git
- init npm to create the package.json file
- add [express](https://www.npmjs.com/package/express) [cors](https://www.npmjs.com/package/cors) [cookie-parser](https://www.npmjs.com/package/cookie-parser) [dotenv](https://www.npmjs.com/package/dotenv) [mysql2](https://www.npmjs.com/package/mysql2) [joi](https://www.npmjs.com/package/joi) packages to create a very basic server
- stage, commit and push the changes to the remote repository in GitHub

```console
git checkout -b step-02-adding-npm-packages
npm init -y
npm install express cors cookie-parser dotenv mysql2 joi
npm install nodemon --save-dev
git add .
git commit -m "add npm packages"
git push --set-upstream origin step-02-adding-npm-packages
```

## Step 3 - Create the server

### branch: [step-03-creating-server](https://github.com/geomarb/backend-example/tree/step-03-creating-server)

#### Changed files

##### `.env`

- edit the `.env` file to add the `PORT` variable

```console
PORT=4000
```

##### [src/app.js](src/app.js)

- require: `express, cors, cookie-parser`
- use middleware: `express.json(), cors(), cookieParser()`
- export `app`

##### [src/index.js](src/index.js)

- call: `dotenv`
- import: `app.js)`
- get the port to run from `process.env.PORT`, if undefined set it as `4000`
- make app listen on the defined port

##### [package.json](package.json)

- create the `dev` script with `nodemon run src/index.js`
- run the server with `npm run dev` and make sure it working, fix any issues

#### git commands

```console
git checkout -b step-03-creating-server
git add .
git commit -m "create server"
git push --set-upstream origin step-03-creating-server
```

## Step 4 - Users and auth endpoints

### branch: [step-04-get-user-route](https://github.com/geomarb/backend-example/tree/step-04-get-user-route)

### Users endpoints

| Method | Endpoint   | Description                     | Path Parameter | Request Query Parameter | Access Control        | Request Body                      | Response Body                          | Response Status |
| ------ | ---------- | ------------------------------- | -------------- | ----------------------- | --------------------- | --------------------------------- | -------------------------------------- | --------------- |
| GET    | /users     | Returns a list with all users   | none           | none                    | adm role only         | none                              | list of users                          | 200             |
| GET    | /users/:id | Returns a single user by its id | id             | id                      | adm role only         | none                              | a single user                          | 200             |
| POST   | /users     | Create a new user               | none           | none                    | adm role only         | New user's name, email            | id, name, email, email                 | 201             |
| PUT    | /users/:id | Update a user                   | id             | id                      | adm role only         | Modified user's name and/or email | Updated name, email and/or role        | 200             |
| DELETE | /users/:id | Delete a user                   | id             | id                      | and role and own user | none                              | id, name and email of the deleted user | 200             |

##### [src/models/user.model.js](src/models/user.model.js)

- create a fake hard coded users data (this will be changed later to get data from the database)
- create the `getUsers` method which will return all users
- create the `getUserById` method which will return one user by the id
- create the `createUser` method which will create a new user and return the user with the id
- create the `updateUser` method which will update the name and/or email of a user and return the updated user
- create the `deleteUser` method which will remove a user by the id and return the deleted user
- create the `login` method which will check if the email exists and if the password id valid then the user or throw an error

##### [src/models/index.js](src/models/index.js)

- import `user.model.js` and export it as `userModel`

##### [src/controllers/user.controller.js](src/controllers/user.controller.js)

- import `userModel` from `models`
- create the `getUsers` method which will process the request and send a response with all users returned from `userModel.getUsers()`
- create the `getUserById` method which will process the request with user's id param and send a response with one user returned from `userModel.getUserById(req.params.id)`
- create the `createUser` method which will process the request with name, email and password in the body, create a new user send a response with the new user returned from `userModel.createUser(req.body)`
- create the `updateUser` method which will process the request with a changed name and/or email of a user and return the updated user
- create the `deleteUser` method which will process the request with an user's id to be deleted and return the deleted user
- create the `deleteUser` method which will process the request with an user's id to be deleted and return the deleted user

##### [src/controllers/index.js](src/controllers/index.js)

- import `user.controller.js` and export it as `userController`

##### [src/routes/user.routes.js](src/routes/user.routes.js)

- create the `router`, import `user.controller`
- create the `GET /` route which calls `userController.getUsers`
- create the `GET /` route which calls `userController.getUsersById`
- create the `POST /` route which calls `userController.createUser`
- create the `PUT /` route which calls `userController.updateUser`
- create the `DELETE /` route which calls `userController.deleteUser`
- export the `router`

### authorization and registration endpoints

| Method | Endpoint  | Description                | Path Parameter | Request Query Parameter | Access Control | Request Body                           | Response Body         | Response Status |
| ------ | --------- | -------------------------- | -------------- | ----------------------- | -------------- | -------------------------------------- | --------------------- | --------------- |
| POST   | /register | Create a new user          | none           | none                    | Anyone         | New user's name, email, role, password | id, name, email, role | 201             |
| POST   | /login    | Create a new login session | none           | none                    | Anyone         | User's email and password              | id, name, email, role | 200             |
| GET    | /logout   | Delete the current session | none           | none                    | Logged user    | none                                   | none                  | 200             |
| GET    | /profile  | Get logged user's profile  | none           | none                    | Logged user    | none                                   | none                  | 200             |
| POST   | /profile  | Update user's profile      | none           | none                    | Logged user    | User's name, email, role               | none                  | 200             |
| PATCH  | /password | Update user's password     | none           | none                    | Logged user    | User's new password                    | none                  | 200             |

##### [src/routes/auth.routes.js](src/routes/auth.routes.js)

- create the `router`, import `user.controller`
- create the `POST /register` route which calls `authController.createUser`
- create the `POST /login` route which calls `authController.login`
- create the `GET /logout` route which calls `authController.logout`
- export the `router`

##### [src/routes/index.js](src/controllers/index.js)

- create the `router`, import `./user.routes.js` as `userRoutes`
- use `userRoutes` in the router under `/users` endpoint
- use `authRoutes` in the router under `/` endpoint
- export `router`

##### [src/app.js](src/app.js)

- import `router` from `./routes`
- use it as a middleware under `/api` route

#### Run the server

- run the server with `npm run dev`
- fix all errors
- go to the route [http://localhost:4000/api/users](http://localhost:4000/api/users).
- check all users were returned

#### git commands

```console
git checkout -b step-04-get-user-route
git add .
git commit -m "create get user route"
git push --set-upstream origin step-04-get-user-route
```
