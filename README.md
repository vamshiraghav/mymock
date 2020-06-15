# Deploy `json-server` to `{{ free hosting site }}`

> Instructions how to deploy the full fake REST API [json-server](https://github.com/zemoso-int/json-mock-server) to various free hosting sites. Should only be used in development purpose but can act as a simpler database for smaller applications.

<img src="https://cms-assets.tutsplus.com/uploads/users/34/posts/27871/preview_image/json.jpg" align="right">

* [**Create your database**](#create-your-database)
* [Deploy to **Heroku**](#deploy-to-heroku)
* [Deploy to **now**](#deploy-to-now)
* [Deploy to **Azure**](#deploy-to-azure)


## Create your database
##  Some of the websites to create your database
1. [`Mockaroo`](https://www.mockaroo.com/)
2. [`Casual Fake data generator`](https://github.com/boo1ean/casual)
3. [`Faker.js`](https://github.com/Marak/faker.js)
4. [`json-generator`](https://www.json-generator.com/)

1 . Clone this repo to anywhere on your computer.

```bash
git clone git@github.com:pall-1024/json-server-heroku.git
```

2 . Change `db.json` to **your own content** according to the [`json-server example`](https://github.com/typicode/json-server#example) and then `commit` your changes to git. 

_this example will create `/posts` route , each resource will have `id`, `title` and `content`. `id` will auto increment!_
```json
{
  "posts":[
    {
      "id" : 0,
      "title": "First post!",
      "content" : "My first content!"
    }
  ]
}
```

---

## Deploy to **Heroku**

<img align="right" width="100px" height="auto" src="https://cdn.worldvectorlogo.com/logos/heroku.svg" alt="Heroku">

Heroku is a free hosting service for hosting small projects. Easy setup and deploy from the command line via _git_.

###### Pros

* Easy setup
* Free

###### Cons

* App has to sleep a couple of hours every day.
* "Powers down" after 30 mins of inactivity. Starts back up when you visit the site but it takes a few extra seconds. Can maybe be solved with [**Kaffeine**](http://kaffeine.herokuapp.com/)

---

### Install Heroku

1 . [Create your database](#create-your-database)

2 . Create an account on <br/>[https://heroku.com](https://heroku.com)

3 . Install the Heroku CLI on your computer: <br/>[https://devcenter.heroku.com/articles/heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)
```bash

$ sudo apt update
$ sudo apt install snapd
for ubuntu sudo snap install --classic heroku

```
4 . Connect the Heroku CLI to your account by writing the following command in your terminal and follow the instructions on the command line:
```bash
heroku login
```

5 . Then create a remote heroku project, kinda like creating a git repository on GitHub. This will create a project on Heroku with a random name. If you want to name your app you have to supply your own name like `heroku create project-name`:
```bash
heroku create my-cool-project
```

6 . Push your app to __Heroku__ (you will see a wall of code)
```bash
git push heroku master
```

7 . Visit your newly create app by opening it via heroku:
```bash
heroku open
```

8 . For debugging if something went wrong:
```bash
heroku logs --tail
```

---

#### How it works

Heroku will look for a startup-script, this is by default `npm start` so make sure you have that in your `package.json` (assuming your script is called `server.js`):
```json
 "scripts": {
    "start" : "node server.js"
 }
```

You also have to make changes to the port, you can't hardcode a dev-port. But you can reference herokus port. So the code will have the following:
```js
const port = process.env.PORT || 4000;
```

---

## Deploy to **now**

1 . [Create your database](#create-your-database)

2 . Install now cli-tool globally
```bash
npm install -g now
```

3 . Run the `now` command in this folder/repo where your project is. If you run it for the first time, you will be prompted to login, after login, run the command again:
```
now --public
```
_`--public` is to skip the prompt telling you that you will open source your project if you deploy it to now_

4 . The URL will be copied automatically and you can just paste it into your browser.

5. **Optional**: Rename the deployment:
```bash
now alias https://your-deployed-name.now.sh new-name
```
_first argument is the deployed site, second argument is the new name to give it_

---

## Deploy to **Azure**

<img align="right" width="100px" height="auto" src="https://docs.microsoft.com/en-us/azure/media/index/azure-germany.svg" alt="Azure">

You can also use _Microsoft Azure_ to deploy a smaller app for free to the Azure platform. The service is not as easy as _Heroku_ and you might go insane because the documentation is really really bad at some times and it's hard to troubleshoot.

The **pros** are that on _Azure_ the app **will not be forced to sleep**. It will sleep automatically on inactivity but you can just visit it and it will start up.

## Installation

1 . Create a Microsoft Account that you can use on Azure: </br>
https://azure.microsoft.com/sv-se/

2 . Install the `azure-cli`: <br/>
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
_This might cause some trouble, you will see. Remember to restart your terminal or maybe your computer if the commands after this does not work_

3 . Login to the service via the command line and follow the instructions: </br>
```bash
az login
```
_You will be prompted to visit a website and paste a confirmation code_


## Create the project

1 . [Create your database](#create-your-database)

2 . Create a resource group for your projects, replace the name to whatever you want just be sure to use the same group name in all commands to come. You only have to create the resource group and service plan once, then you can use the same group and plan for all other apps you create if you like.

```bash
az group create -n NameOfResourceGroup -l northeurope
```

3 . Create a service plan:

```
az appservice plan create -n NameOfServicePlan -g NameOfResourceGroup
```

4 . Create the actual app and supply the service plan and resource group
```bash
az appservice web create -n NameOfApp -g NameOfResourceGroup --plan NameOfServicePlan
```

5 . Create deployment details. A git-repo is not created automatically so we have to create it with a command:

```bash
az appservice web source-control config-local-git -g NameOfApp -g NameOfResourceGroup
```

6 . From the command in step 5 you should get a **url** in return. Copy this url and add it as a remote to your local git project, for example:

```bash
git remote add azure https://jesperorb@deploy-testing.scm.azurewebsites.net/deploy-testing.git
```

7 . Now you should be able to push your app:
```bash
git push azure master
```

You should be prompted to supply a password, this should be the pass to your account. If not, you can choose a different password at your Dashboard for Azure: **[https://portal.azure.com/](https://portal.azure.com/)**

Choose **App Services** in the sidebar to the left and the choose your app in the list that appears then go to **Deployment Credentials** to change your password for deployment.

# USING JSON SERVER
## Table of contents

<!-- toc -->

- [Getting started](#getting-started)
- [Routes](#routes)
  * [Plural routes](#plural-routes)
  * [Singular routes](#singular-routes)
  * [Filter](#filter)
  * [Paginate](#paginate)
  * [Sort](#sort)
  * [Slice](#slice)
  * [Operators](#operators)
  * [Full-text search](#full-text-search)
  * [Relationships](#relationships)
  * [Database](#database)
<!-- tocstop -->
## Getting started

Install JSON Server 

```
npm install -g json-server
```

Create a `db.json` file with some data

```json
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

Start JSON Server

```bash
json-server --watch db.json
```



Now if you go to [http://localhost:3000/posts/1](http://localhost:3000/posts/1), you'll get

```json
{ "id": 1, "title": "json-server", "author": "typicode" }
```

Also when doing requests, it's good to know that:

- If you make POST, PUT, PATCH or DELETE requests, changes will be automatically and safely saved to `db.json` using [lowdb](https://github.com/typicode/lowdb).
- Your request body JSON should be object enclosed, just like the GET output. (for example `{"name": "Foobar"}`)
- Id values are not mutable. Any `id` value in the body of your PUT or PATCH request will be ignored. Only a value set in a POST request will be respected, but only if not already taken.
- A POST, PUT or PATCH request should include a `Content-Type: application/json` header to use the JSON in the request body. Otherwise it will return a 2XX status code, but without changes being made to the data. 

## Routes

Based on the previous `db.json` file, here are all the default routes. You can also add [other routes](#add-custom-routes) using `--routes`.

### Plural routes

```
GET    /posts
GET    /posts/1
POST   /posts
PUT    /posts/1
PATCH  /posts/1
DELETE /posts/1
```

### Singular routes

```
GET    /profile
POST   /profile
PUT    /profile
PATCH  /profile
```

### Filter

Use `.` to access deep properties

```
GET /posts?title=json-server&author=typicode
GET /posts?id=1&id=2
GET /comments?author.name=typicode
```

### Paginate

Use `_page` and optionally `_limit` to paginate returned data.

In the `Link` header you'll get `first`, `prev`, `next` and `last` links.


```
GET /posts?_page=7
GET /posts?_page=7&_limit=20
```

_10 items are returned by default_

### Sort

Add `_sort` and `_order` (ascending order by default)

```
GET /posts?_sort=views&_order=asc
GET /posts/1/comments?_sort=votes&_order=asc
```

For multiple fields, use the following format:

```
GET /posts?_sort=user,views&_order=desc,asc
```

### Slice

Add `_start` and `_end` or `_limit` (an `X-Total-Count` header is included in the response)

```
GET /posts?_start=20&_end=30
GET /posts/1/comments?_start=20&_end=30
GET /posts/1/comments?_start=20&_limit=10
```

_Works exactly as [Array.slice](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) (i.e. `_start` is inclusive and `_end` exclusive)_

### Operators

Add `_gte` or `_lte` for getting a range

```
GET /posts?views_gte=10&views_lte=20
```

Add `_ne` to exclude a value

```
GET /posts?id_ne=1
```

Add `_like` to filter (RegExp supported)

```
GET /posts?title_like=server
```

### Full-text search

Add `q`

```
GET /posts?q=internet
```

### Relationships

To include children resources, add `_embed`

```
GET /posts?_embed=comments
GET /posts/1?_embed=comments
```

To include parent resource, add `_expand`

```
GET /comments?_expand=post
GET /comments/1?_expand=post
```

To get or create nested resources (by default one level, [add custom routes](#add-custom-routes) for more)

```
GET  /posts/1/comments
POST /posts/1/comments
```

### Database

```
GET /db
```
