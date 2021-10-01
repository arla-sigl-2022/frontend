# Frontend Workshop

This is the subject of the frontend workshop for students of EPITA - SIGL 2022.

The aim of the workshop is to implement a user interface (UI) for Garlaxy.
To implement it, we will use:

- [NodeJS](https://nodejs.org/en/about/): to be able to use dependencies (other developper's code)
- [PureCSS](https://purecss.io/): to have some small help on making our application responsive
- [ReactJS](https://fr.reactjs.org/): to manage DOM elements rendering and local component state
- [Webpack](https://webpack.js.org/): to bundle all our source files into a single `JavaScript` file (that will be our production artifact)

We are doing only the user interface without any data persistency...for now!

## Step 0: Requirements

### IDE

We strongly encourage you to use [Visual Studio Code](https://code.visualstudio.com) for your frontend project.
It's totally free and open source (even written in TypeScript!).

You don't have to install any extra plugins for this workshop.

### Install NodeJS

You need to install NodeJS (a.k.a `node`) on your local machine.

Because NodeJS is a tool that evolves fast and has multiple versions, we will use a versionning tool to help us using multiple versions, depending on the project we're working on: [NVM](https://github.com/nvm-sh/nvm) (Node Version Manager)

To install `nvm`, follow instructions in the README of the project: https://github.com/nvm-sh/nvm#installing-and-updating

Once `nvm` is install:

- Create a file `.nvmrc` with `v16` inside
- Install node v14 using `nvm`: `nvm install v16`

Then, everytime you work on your project, you can type `nvm use` command, and it will switch you to the version inside the `.nvmrc` file.

You can verify if everything is fine by checking node version: `node -v`
and it should output version 16.

## Step 1: Make me responsive!

For this part, you will only code HTML and CSS.

- Clone the [responsive-demo](https://github.com/arla-sigl-2022/responsive-demo) on another directory on your machine
- Go into the responsive-demo folder, and run:

```sh
# Make sure you are using NodeJS version 16.
# If you have nvm; just run nvm use
npm install
npm start
```

- Open your browser on [localhost:8080](http://localhost:8080)

This project is using 2 sources of style:

- [PureCSS](https://purecss.io/)
- [Custom garlaxy.css file](https://github.com/arla-sigl-2022/responsive-demo/blob/main/garlaxy.css)
- [Our galaxy as the page's background](https://github.com/arla-sigl-2022/responsive-demo/blob/main/garlaxy.jpg)

We use PureCSS just by referencing a public URL which expose the minified CSS code of pure.
[See index.html file's <link> tags](https://github.com/arla-sigl-2022/responsive-demo/blob/d41617879fb816b9f6276b3331ea6036f266aaf6/index.html#L7)

> Note: garlaxy is the last one for a reason. Browser reads html file from top to bottom. So if you want to override properties from the pureCSS's css file, you need to place your garlaxy file afterwards.

About the app layout:

- [Header](https://github.com/arla-sigl-2022/responsive-demo/blob/d41617879fb816b9f6276b3331ea6036f266aaf6/index.html#L18) and [Footer](https://github.com/arla-sigl-2022/responsive-demo/blob/d41617879fb816b9f6276b3331ea6036f266aaf6/index.html#L96) takes 10% of the viewport height (10vh)
- [Content](https://github.com/arla-sigl-2022/responsive-demo/blob/d41617879fb816b9f6276b3331ea6036f266aaf6/index.html#L21) of the app takes 80% of the viewport height. We are using [PureCSS responsive grids](https://purecss.io/grids/) to handle content layout.

About the responsive:

- The template uses [media queries](https://www.w3schools.com/css/css_rwd_mediaqueries.asp) which is native CSS, so built-in with browsers. See [how the template uses media queries](https://github.com/arla-sigl-2022/responsive-demo/blob/d41617879fb816b9f6276b3331ea6036f266aaf6/garlaxy.css#L102)

Feel free to take few minutes to change the style of garlaxy, by editing [garlaxy.css](https://github.com/arla-sigl-2022/responsive-demo/blob/main/garlaxy.css) on your local host.
Here are some usefull links:

- [Matrial Color Tool](https://material.io/resources/color/#!/?view.left=0&view.right=0): pick colors that goes well together
- [PureCSS buttons](https://purecss.io/buttons/) and [PureCSS tables](https://purecss.io/tables/): docs for component this template is using
- [Extend PureCSS](https://purecss.io/extend/): to customize a bit the default design of PureCSS component.
- [I don't want to use any css framework, let me use Flexbox!](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox): This is what PureCSS is based on. Flexboxes in CSS are the main component to help you build responsive website.

## Step 2: Make me reactive!

Sofar, we have a responsive application, but we don't handle any user interactions (menu selection or button clicks).

### Create your ReactJS project

[ReactJS](https://fr.reactjs.org/) is a JavaScript framework from FaceBook.
We chose this since it seems to be the most notorious one, when checking [NodeJS framework trends](https://www.npmtrends.com/@angular/core-vs-angular-vs-react-vs-vue-vs-svelte) with its competitors.

Facebook has lots of project in ReactJS. So they published and maintain a project called [create-react-app](https://create-react-app.dev/). This project is use to generate a fresh new frontend project using react and other tools that matches the react eco-system.

- From your project's repository (e.g. groupe-13), create a new react app using [create-react-app](https://create-react-app.dev/) by typing the following commands in your terminal:

```sh
# From your project's repository where you should have a Dockerfile and an index.html file

# should use node version 16
nvm use v16

# This will create a new frontend repository
npx create-react-app frontend --use-npm

# move to your frontend directory
cd frontend

# install another dependency (for our menu)
npm install --save react-router-dom

# You need to commit your changes, before ejecting the project
cd ../
git add .
git commit -m "before eject"

# move back inside your frontend project
cd frontend
npm run eject
# You should be all set!
```

Congrats, we have a fresh new ReactJS project!

Our commands has generated quite many files. Don't worry, and don't try to understand all the files that has been generated, we will get there!

You should have the following tree inside `frontend` folder (ignoring node_modules):

```sh
.
├── README.md
├── config                      # All configurations for packaging the app and setup tests
│   ├── env.js
│   ├── getHttpsConfig.js
│   ├── jest
│   │   ├── babelTransform.js
│   │   ├── cssTransform.js
│   │   └── fileTransform.js
│   ├── modules.js
│   ├── paths.js
│   ├── pnpTs.js
│   ├── webpack.config.js
│   └── webpackDevServer.config.js
├── package-lock.json           # Freezes the exact dependencies you've installed. Other developers will then get same versions
├── package.json                # Your NodeJS project configuration file.
├── public                      # Static files that will stay unchanged when building the app
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
├── scripts                     # Scripts to build, start and test your app locally
│   ├── build.js
│   ├── start.js
│   └── test.js
└── src                         # Your ReactJS source code. This is where magic will happen
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js        # your entry point that links your html to yout JS code.
    ├── logo.svg
    ├── reportWebVitals.js
    └── setupTests.js

5 directories, 30 files

```

You can run the template of react:

```sh
# from frontend/
nvm use v16
npm start
```

This should automatically opens your browser son [localhost:3000](http://localhost:3000).

Now every time you will save code changes, the page on your browser will automatically reload with the new verstion.

### Add PureCSS to your ReactJS's repository

We will merge your responsive template from Step 1 to the code we just generated.

- Add responsive template links to `frontend/public/index.html` inside the header's section:
```html
<link
  rel="stylesheet"
  href="https://unpkg.com/purecss@2.0.6/build/pure-min.css"
  integrity="sha384-Uu6IeWbM+gzNVXJcM9XV3SohHtmWE+3VGi496jvgX1jyvDTXfdK+rfZc8C1Aehk5"
  crossorigin="anonymous"
/>
<link
  rel="stylesheet"
  href="https://unpkg.com/purecss@2.0.6/build/grids-responsive-min.css"
/>
```

- Rename title to Garlaxy

```html
<title>Garlaxy</title>
```

- Replace lang from `en` to `fr`:
```html
<html lang="fr">
```

- Copy your background `garlaxy.jpg` image to `frontend/src/garlaxy.jpg` image

- Convert content of your responsive's template html body in React's JSX syntax:

```jsx
// In your frontend/src/App.js
// - paste between <> </> the html code
// - rename class= to className=
// That's it!

// your app should look like (content is truncated here)
function App() {
  return (
    <>
      <div class="header">
        <p class="garlaxy-title text-centered">GARLAXY</p>
      </div>
      <div class="content">...</div>
      <div class="footer">
        <p class="text-centered">
          {" "}
          Besoin d'aide? <a href="mailto:groupe13@arla-sigl.fr">
            Contactez-nous
          </a>{" "}
        </p>
      </div>
    </>
  );
}
```
- Replace the content of `frontend/src/App.css` with the content of `garlaxy.css` of your responsive template
- Go back on your [browser locahost:3000](http://localhost:3000), and you should see your template, still inactive
> You can leave the `npm start` command running the whole workshop, it should'nt be an issue.
> If it crashes, just start it again.

### Make your menu reactive

You will create two other views, the "Panier" and the "Commandes" view.

You will use [react-router-dom](https://reactrouter.com/web/guides/quick-start) JavaScript library to implement it.

Copy the following files to your `frontend/src/` folder:
- [Commande.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/Commandes.js)
- [Content.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/Content.js)
- [Footer.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/Footer.js)
- [Header.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/Header.js)
- [Panier.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/Panier.js)
- [Ressources.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/Ressources.js)
- [SideMenu.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/SideMenu.js)

And adapt your `frontend/src/App.js` file like [this App.js file](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/App.js)

Usage of react router is happening inside Content.js and SideMenu.js file.

> Note: you have an example of how to pass ReactJS props from a parent to a child component
> in [SideMenu.js](https://github.com/arla-sigl-2022/groupe-13/blob/5e55b1968f48e19858e6f395023e3eed7235603b/frontend/src/SideMenu.js)

You should now have a fonctional menu!
Check it out on [localhost:3000](http://localhost:3000)

## Step 3: Adapt our CD

We need to adapt our nginx config to [solve the known problem of react-router with Nginx](https://stackoverflow.com/questions/43951720/react-router-and-nginx).

Create a new nginx folder and create a default.conf nginx file with the following content:
```plain
# inside frontend/nginx/default.conf
server {
    
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        try_files  $uri /index.html;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

- Create a new Dockerfile under frontend/ directory:
```dockerfile
# Inside frontend/Dockerfile
FROM node:16.10.0-slim as build

ADD . /code
WORKDIR /code
RUN npm ci
RUN npm run build

FROM nginx:1.21.3

ADD nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /code/build /usr/share/nginx/html
```
- create a copy of `frontend/.gitignore` to `frontend/.dockerignore` file. This will make sure we are not mounting dirty files/folders when building the docker image
```sh
# from frontend/
cp .gitignore .dockerignore
```

> Note: this dockerfile uses this nice [multi-stage build feature from Docker](https://docs.docker.com/develop/develop-images/multistage-build/)

- Running `npm run build` will create the frontend/build/ folder that will contain all our static JS, HTML, CSS and media files that will be serve by Nginx

- Try out to run the docker container locally:
```sh
# from frontend/
docker build -t garlaxy:react .
docker run -p 80:8090 garlaxy:react
```

Check out your app on [localhost:8090](http://localhost:8090).

- Adapt your github workflow to build your docker image from inside `frontend` folder, and you should be set!
Just add `working-directory: frontend` to your build and publish step.
```yaml
# Inside .github/workflow/main.yml
build-frontend: # you can rename your build step to be more precise!
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      - name: build and publish frontend docker image
        working-directory: frontend
        run: | 
          ...
```

- Now just commit and push your changes on main, and you should have your new react app deployed, yeay!

> Note:: Feel free to compare your changes with [our step 2 to step 3 changes](https://github.com/arla-sigl-2022/groupe-13/pull/1/commits/013e3860cdbeeceb25fdb4daf778998e26d31a03)

## Step 4: Build the "Panier" feature

Feature to implement: When user clicks on “Acheter”, we want his article to be displayed in “Panier” section

### Create a context for your application

We will create a context where we will put our ressources as JSON object (like a web API would return).

- Copy garlaxy context file inside `frontend/src/` folder: [GarlaxyContext.js](https://github.com/arla-sigl-2022/groupe-13/blob/a25c63c4daabaa6d17137003f8f0883865d118aa/frontend/src/GarlaxyContext.js)
- Adapt your `frontend/src/App.js` file to setup the context for all contents: [App.js with context](https://github.com/arla-sigl-2022/groupe-13/blob/a25c63c4daabaa6d17137003f8f0883865d118aa/frontend/src/App.js)

Now, you will modify Panier.js and Ressources.js to load items from the context, and dispatch add and remove event when user clicks on buttons:
- [Panier.js](https://github.com/arla-sigl-2022/groupe-13/blob/a25c63c4daabaa6d17137003f8f0883865d118aa/frontend/src/Panier.js)
- [Ressources.js](https://github.com/arla-sigl-2022/groupe-13/blob/a25c63c4daabaa6d17137003f8f0883865d118aa/frontend/src/Ressources.js)

> Note: You can check out [all diffs from Step 3 to Step 4](https://github.com/arla-sigl-2022/groupe-13/pull/1/commits/a25c63c4daabaa6d17137003f8f0883865d118aa)

You should be all set on [localhost:3000](http://localhost:3000), and be able to interact with buttons!

> Note: you can deploy by just pushing all your files on main!

## Challenge (optional): Add badge to your Panier

Here is css code to create a badge:

```css
.badge {
  background-color:rgba(153, 0, 254, 1);
  color:#fff;
  display: inline-block;
  padding-left:8px;
  padding-right:8px;
  text-align:center;
  border-radius:50%;
}
```

Just use it inside JSX component like 
```jsx
<span className="badge">3</span>
```

> Note: If you're fedup, you can [see correction here](https://github.com/arla-sigl-2022/groupe-13/commit/a25c63c4daabaa6d17137003f8f0883865d118aa#diff-1cf387c012cd2d02f1923e589f86df4a4802427057dbc90f47b87f04d29d54bc)