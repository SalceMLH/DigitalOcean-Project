# Foreword #

In this tutorial, we are building a simple backend with NodeJS to serve the static files, and the frontend with HTML, CSS, and javascript.

----

# 1. Creating your base project #

## 1.1. Creating the project folder ##

Assuming you already installed NodeJS on your machine, create a directory to hold your application.

```
mkdir mlh-localhost-digital-ocean
cd mlh-localhost-digital-ocean
```

## 1.2. Adding the Package.json ##

Use the npm init command to create a `package.json` for your application.

```
npm init
```

This command prompts you for many things, such as the name and version of your application. For now, you can merely hit Enter to accept the defaults for them all.

## 1.3. Adding express to serve your files ##

Now you need to install Express in the mlh-localhost-digital-ocean directory and save it in the dependencies list.

```
npm install express --save
```

## 1.4. Creating your Index.html ##

Create your `public` folder to host your static files

```
mkdir public
```

Create your `index.html` inside your `public` folder with the content

```
<h1>Hello World!</h1>
```

## 1.5. Serving your index.html ##

Inside your root folder create your `index.js` file with the following content:

```
const express = require('express');
const app = express();
const port = 3000;
const request = require('request')
const fs = require("fs");

app.use(express.static('public'));

const path = require('path');

app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'public/index.html'));
})

app.listen(port, () => console.log(`Digital Ocean app listening on port ${port}!`))
```

And in your `package.json` inside the key `scripts` add the following:

```json
"start": "node index.js",
```

Inside the root folder run 
```
npm start
```

Now on your browser when you access `http://localhost:3000/` you will see a nice and simple `Hello World`

## 2.1. Creating the HTML structure ##

Replace the content of your `public/index.html` with:

```html
<html>
  <head>
    <script src="//code.jquery.com/jquery-1.11.1.min.js"></script>
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.0/css/bootstrap.min.css" rel="stylesheet" id="bootstrap-css">
    <script src="//maxcdn.bootstrapcdn.com/bootstrap/3.3.0/js/bootstrap.min.js"></script>

    <link rel="stylesheet" type="text/css" href="/css/index.css" />
    <link rel="stylesheet" type="text/css" href="/css/carousel.css" />
    <script src="/js/app.js"></script>
    <script src="/js/carousel.js"></script>
  </head>

  <body>
    <div class="logo-container">
      <a href="https://mlh.io">
        <img class="logo" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9e/Major_League_Hacking_logo.svg/1200px-Major_League_Hacking_logo.svg.png" />
      </a>
    </div>

    <div class="filter-container">
      <div class="secondary-filter-container">
        <input id="filter" type="text" placeholder="Filter your beach or clean up here"/>
      </div>
    </div>

    <h2>Beaches</h2>
    <div class="container">
      <div class="row">
        <div class="MultiCarousel" data-items="3,3,3,3" data-slide="3" id="beach-locations-carousel"  data-interval="1000">
          <div class="MultiCarousel-inner" id="beaches-container">
          </div>
          <button class="leftLst">
            <i class="arrow left"></i>
          </button>
          <button class="rightLst">
            <i class="arrow right"></i>
          </button>
        </div>
      </div>
    </div>

    <h2>Clean up dates</h2>
    <div id="clean-ups-container">
    </div>
  </body>
</html>
```

A few things are important to notice:

1. We are importing JQuery and Bootstrap to show a Carousel; you should see it working soon.
2. We are creating the basic HTML structure which includes the logo and the containers to display both the beaches and clean-ups locations

----

## 1. Working with styles ##

### 2.2.1. Adding the general styles ###

Inside your `public` folder, create an `css` folder to host your stylesheets

```
mkdir public/css
```

Inside your `public/css` folder create a file named `index.css` with the following content

```css
* {
  outline: none
}

body {
  background-color: #F7F9F9;
  text-align: center;
}

.filter-container {
  margin-bottom: 50px;
}

.logo-container, .filter-container {
  display: flex;
  justify-content: center;
}

.logo {
  padding-top: 150px;
  width: 272px;
}

.secondary-filter-container {
  border: solid silver 1px;
  border-radius: 25px;
  margin-top: 30px;
}

#filter {
  border: none;
  border-radius: 25px;
  font-size: 16px;
  height: 40px;
  outline:none;
  padding-left: 20px;
  width: 582px;
}

.card {
  background: #fff;
  border-radius: 5px;
  box-shadow: 0 2px 10px 0 rgba(0,0,0,0.08);
  color: black;
  display: inline-block;
  font-size: 13px;
  height: 125px;
  overflow: hidden;
  padding: 10px;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.card.beach {
  width: 100%;
  margin-bottom: 30px;
}

.card.clean-up {
  height: 125px;
  margin: 10px;
  width: 305px;
}

.card-details {
  display: block;
}

.card-details:first-child {
  margin-top: 15px;
}

.subscribe-container {
  margin-top: 15px;
}

.subscribe-btn {
  background: #f7db2f;
  border: 1px solid #fefa7f;
  border-radius: 3px;
  color: #343434;
  font-size: 1.1em;
  font-weight: bold;
  text-decoration: none;
}

.subscribe-input {
  width: 241px;
}

.thanks {
  color: #337ab7;
  display: block;
  margin-top: 10px
}

.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

### 2.2.2. Adding the carousel styles ###

Inside your `public/css` folder create a file named `carousel.css` with the following content
```css
.MultiCarousel {
  float: left;
  overflow: hidden;
  padding: 15px;
  width: 100%;
  position:relative;
  margin-top: 50px;
}

.MultiCarousel .MultiCarousel-inner {
  transition: 1s ease all;
  float: left;
  height: 140px;
  overflow: hidden;
}

.MultiCarousel .MultiCarousel-inner.clean-ups{
  height: 145px;
}

.MultiCarousel .MultiCarousel-inner .item {
  float: left;
}

.MultiCarousel .leftLst, .MultiCarousel .rightLst {
  position:absolute;
  border-radius:50%;
  top: 63px;
  width: 40px;
  height: 40px;
  border: 0;
  background-color: transparent;
}

.MultiCarousel .leftLst {
  left: -5px;
}

.MultiCarousel .rightLst {
  right: -5px;
}

.MultiCarousel .leftLst.over, .MultiCarousel .rightLst.over {
  display: none;
}

i {
  border: solid black;
  border-width: 0 3px 3px 0;
  display: inline-block;
  padding: 3px;
}

.right {
  transform: rotate(-45deg);
  -webkit-transform: rotate(-45deg);
}

.left {
  transform: rotate(135deg);
  -webkit-transform: rotate(135deg);
}

.item-container {
  padding: 0 10px;
}
```

----

## 2. Working with javascript ##

### 2.3.1. Adding the general logic ###

Inside your `public` folder, create an `js` folder to host your stylesheets

```
mkdir public/js
```

Inside your `public/js` folder create a file named `app.js` with the following content

```js
let beachesLocations;
let cleanUpsLocations;

const getBeachesContainer = () => document.getElementById("beaches-container");
const getCleanUpsContainer = () => document.getElementById("clean-ups-container");
const getFilter = () => document.getElementById('filter');

const clearContainer = (container) => container.innerHTML = '';

const showBeaches = (beaches) => {
  const beachesContainer = getBeachesContainer();
  clearContainer(beachesContainer);

  beaches.forEach((beach) => {
    const cardContainer = document.createElement("div");
    cardContainer.classList.add("item");
    cardContainer.classList.add("item-container");

    const card = document.createElement("div");
    card.classList.add("card");
    card.classList.add("beach");
    cardContainer.appendChild(card);

    const beachName = document.createElement("span");
    beachName.innerHTML = beach.NameMobileWeb;
    card.appendChild(beachName);

    const beachLocation = document.createElement("span");
    beachLocation.innerHTML = beach.LocationMobileWeb;
    beachLocation.classList.add("card-details");
    beachLocation.classList.add("truncate");
    card.appendChild(beachLocation);

    const county = document.createElement("span");
    county.innerHTML = beach.COUNTY;
    county.classList.add("card-details");
    card.appendChild(county);

    const subscribeContainer = document.createElement("div");
    subscribeContainer.classList.add("subscribe-container");
    card.appendChild(subscribeContainer);

    const subscribeInput = document.createElement("input")
    subscribeInput.placeholder = "Email me on the next clean up date"
    subscribeInput.classList.add("subscribe-input");
    subscribeContainer.appendChild(subscribeInput);

    const afterSubscribeMessage = document.createElement("span");
    afterSubscribeMessage.innerHTML = 'Thanks for subscribing to the next clean up date!';
    afterSubscribeMessage.classList.add("thanks");

    const subcribeButton = document.createElement("button");
    subcribeButton.classList.add("subscribe-btn");
    subcribeButton.innerHTML = 'Send';
    subscribeContainer.appendChild(subcribeButton);

    subcribeButton.onclick = () => {
      subscribeInput.remove()
      subcribeButton.remove()
      subscribeContainer.appendChild(afterSubscribeMessage);
    }

    beachesContainer.appendChild(cardContainer);
  });

  activateCarousel();
}

const showCleanUps = (cleanUps) => {
  const cleanUpsContainer = getCleanUpsContainer();
  clearContainer(cleanUpsContainer);

  cleanUps.forEach((cleanUp) => {
    const card = document.createElement("div");
    card.classList.add("card");
    card.classList.add("clean-up");
    cleanUpsContainer.appendChild(card);

    const organization = document.createElement("span");
    organization.innerHTML = cleanUp.organization;
    organization.classList.add("card-details");
    card.appendChild(organization);

    const website = document.createElement("a");
    website.innerHTML = cleanUp.website || '';
    website.href = cleanUp.website;
    website.target = 'blank';
    website.classList.add("truncate");
    website.classList.add("card-details");
    card.appendChild(website);

    const county = document.createElement("span");
    county.innerHTML = cleanUp.county_region;
    county.classList.add("card-details");
    card.appendChild(county);

    const date = document.createElement("span");
    date.innerHTML = (new Date(cleanUp.cleanup_date)).toLocaleDateString();
    date.classList.add("card-details");
    card.appendChild(date);

    const time = document.createElement("span");
    time.innerHTML = cleanUp.start_time ? (cleanUp.start_time +  ' - ' + cleanUp.end_time) : '';
    time.classList.add("card-details");
    card.appendChild(time);
  });
};

const fetchBeachesInformation = () => {
  const url = "https://api.coastal.ca.gov/access/v1/locations";
  fetch(url)
    .then((response) => response.json())
    .then((data) => {
      beachesLocations = data;
      showAllBeaches();
    });
};

const fetchCleanUpsInformation = () => {
  const url = "https://api.coastal.ca.gov/ccd/v1/locations";
  fetch(url)
    .then((response) => response.json())
    .then((data) => {
      cleanUpsLocations = data.filter((c) => c.cleanup_date && c.county_region);
      showAllCleanUps();
    });
};

const showAllBeaches = () => {
  showBeaches(beachesLocations);
}

const showAllCleanUps = () => {
  showCleanUps(cleanUpsLocations);
}

const filterLocations = ()  => {
  const filterValue = getFilter().value;

  const filteredBeaches = beachesLocations.filter((beach) => {
    return beach.NameMobileWeb.toLowerCase().includes(filterValue.toLowerCase()) ||
    beach.COUNTY.toLowerCase().includes(filterValue.toLowerCase())
  });

  showBeaches(filteredBeaches);

  const filteredCleanUps = cleanUpsLocations.filter((beach) => {
    return beach.county_region.toLowerCase().includes(filterValue.toLowerCase());
  });

  showCleanUps(filteredCleanUps);
}

window.onload = () => {
  fetchBeachesInformation();
  fetchCleanUpsInformation();

  getFilter().onkeyup = filterLocations
}
```

### 2.3.2. Adding the carousel logic ###

Inside your `public/js` folder create a file named `carousel.js` with the following content

```js
function activateCarousel() {
    var itemsMainDiv = ('.MultiCarousel');
    var itemsDiv = ('.MultiCarousel-inner');
    var itemWidth = "";

    $('.leftLst, .rightLst').click(function () {
        var condition = $(this).hasClass("leftLst");
        if (condition)
            click(0, this);
        else
            click(1, this)
    });

    ResCarouselSize();

    $(window).resize(function () {
        ResCarouselSize();
    });

    //this function define the size of the items
    function ResCarouselSize() {
        var incno = 0;
        var dataItems = ("data-items");
        var itemClass = ('.item');
        var id = 0;
        var btnParentSb = '';
        var itemsSplit = '';
        var sampwidth = $(itemsMainDiv).width();
        var bodyWidth = $('body').width();
        $(itemsDiv).each(function () {
            id = id + 1;
            var itemNumbers = $(this).find(itemClass).length;
            btnParentSb = $(this).parent().attr(dataItems);
            itemsSplit = btnParentSb.split(',');
            $(this).parent().attr("id", "MultiCarousel" + id);

            if (bodyWidth >= 1200) {
                incno = itemsSplit[3];
                itemWidth = sampwidth / incno;
            }
            else if (bodyWidth >= 992) {
                incno = itemsSplit[2];
                itemWidth = sampwidth / incno;
            }
            else if (bodyWidth >= 768) {
                incno = itemsSplit[1];
                itemWidth = sampwidth / incno;
            }
            else {
                incno = itemsSplit[0];
                itemWidth = sampwidth / incno;
            }
            $(this).css({ 'transform': 'translateX(0px)', 'width': itemWidth * itemNumbers });
            $(this).find(itemClass).each(function () {
                $(this).outerWidth(itemWidth);
            });

            $(".leftLst").addClass("over");
            $(".rightLst").removeClass("over");
          if (itemNumbers <= 3) {
            $(".rightLst").addClass("over");
          }
        });
    }


    //this function used to move the items
    function ResCarousel(e, el, s) {
        var leftBtn = ('.leftLst');
        var rightBtn = ('.rightLst');
        var translateXval = '';
        var divStyle = $(el + ' ' + itemsDiv).css('transform');
        var values = divStyle.match(/-?[\d\.]+/g);
        var xds = Math.abs(values[4]);
        if (e == 0) {
            translateXval = parseInt(xds) - parseInt(itemWidth * s);
            $(el + ' ' + rightBtn).removeClass("over");

            if (translateXval <= itemWidth / 2) {
                translateXval = 0;
                $(el + ' ' + leftBtn).addClass("over");
            }
        }
        else if (e == 1) {
            var itemsCondition = $(el).find(itemsDiv).width() - $(el).width();
            translateXval = parseInt(xds) + parseInt(itemWidth * s);
            $(el + ' ' + leftBtn).removeClass("over");

            if (translateXval >= itemsCondition - itemWidth / 2) {
              console.log('pepa pig')
                translateXval = itemsCondition;
                $(el + ' ' + rightBtn).addClass("over");
            }
        }
        $(el + ' ' + itemsDiv).css('transform', 'translateX(' + -translateXval + 'px)');
    }

    //It is used to get some elements from btn
    function click(ell, ee) {
        var Parent = "#" + $(ee).parent().attr("id");
        var slide = $(Parent).attr("data-slide");
        ResCarousel(ell, Parent, slide);
    }
}
```

----

# 3. Deploying to DigitalOcean #

Now it is time to deploy to DigitalOcean, but before that we need to do a few changes in our application.


## 3.1. Introduction ##

Now that you have your application running, it is time to deploy it to a remote server to allow your friends to access it and see the beaches and clean-ups locations. One of the easiest ways to achieve it is using a Linux server, and this is where [DigitalOcean](digitalocean.com) comes to help.

## 3.2. Creating your account ##

Go to [Digital Ocean](https://www.digitalocean.com/) and create your account. If after creating the account, they ask you a feel questions about your first project, feel free to go ahead an answer them. 

## 3.3. Creating your first server ##

Once logged in you should click in the dropdown menu Create and once it shows the options, click in Droplets. It will redirect you to another page in which you can choose an image. Click in Market Place and look for the NodeJS one. When selecting the machine configuration, feel free to pick the smallest one, which at the moment of this writing is the 1GB / 1 CPU one. You don't need any other configuration, feel free to go straight to Create Droplet. Feel free to ignore all the other configurations since they are optional, go straight to Create Droplet and after you are redirected to another page just wait a couple of minutes until it is created before moving forward.

## 3.4. Creating your deploy user ##

After you create the Droplet, Digital Ocean will send you an email with your credentials, and there you will have your IP, Username, and password. 

Now go to your terminal, and run:

```cmd
ssh root@<THE_IP_IN_YOUR_EMAIL>
```

The command below will be something like

```cmd
ssh root@68.183.227.110
```

It will ask your password; please copy and past the one available on your email. After you type your current password it will ask you for a new one; it only happens on your first time. 

As you may have noticed you're using the root user, this is not a good practice since this user has just too much power. Let's create a common user named `deploy`.

```cmd
adduser deploy
```

It will ask you for a password; please put a different from the first one. After that, it will ask for some personal data like name and phone, feel free to press enter.

Now that the new user exists we need to give him sudo rights so he can perform operations like update and upgrade.

```cmd
usermod -aG sudo deploy
 ```

To log out from the root session
```cmd
exit
```

Now log in with your new user.

```cmd
ssh deploy@<THE_IP_IN_YOUR_EMAIL>
```

## 3.5. Updating and Upgrading your server ##


You need to update and upgrade your server to assure you have the latest packages. On your server console run:

```cmd
sudo apt-get update
```
If it ask you for the `sudo` password, just type your user password.

Now let's upgrade

```cmd
sudo apt-get upgrade 
```

If it asks you for permission, just type `y`

## 3.6. Opening the port for your application ##

Your server is ready to receive an application, but to access it, we need to open the port 3000, which is the one we're using for the application.

On your server run

```cmd 
sudo ufw allow 3000
```

## 3.7. Uploading your project ##

Now that you have your server ready let's upload your project.

Before you go to the next step I recommend you to remove your local `node_modules` folder to avoid the waiting time to upload them all to the server.

Go to the folder that your project is located and run the following command:

```cmd
scp -r mlh-localhost-digital-ocean/ deploy@<THE_IP_IN_YOUR_EMAIL>:/home/deploy
```

Type your password,  and wait a couple of minutes, and once it finishes, you should have your project folder on the server.

## 3.8. Installing the project dependencies ##

On your server go to your project folder and install the node dependencies

```
cd mlh-localhost-digital-ocean
npm install
```

That's all folks!

## 3.9. Running the application ##

We're all set! Let's run the application now

```cmd
npm start
```

Now go to your browser and type `<THE_IP_IN_YOUR_EMAIL>:3000`
