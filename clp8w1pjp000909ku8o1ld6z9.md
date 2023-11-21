---
title: "Host your own Google Maps: A step-by-step guide to hosting and serving navigation, location, and mapping services"
datePublished: Tue Nov 21 2023 22:09:21 GMT+0000 (Coordinated Universal Time)
cuid: clp8w1pjp000909ku8o1ld6z9
slug: host-your-own-google-maps-navigation-location-and-mapping-services
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/1lfI7wkGWZ4/upload/b09a08c2bba2960a0d6d9972d73ec723.jpeg
tags: tutorial, javascript, web-development, nodejs

---

Heard of Google Maps? Want to know how to implement similar maps and host them yourself or make them available offline? This sounds very niche and specific but if that is the case — you are in the right place!

![](https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExd2txcm9oaXl1Nmxsd3h6NzhlMHRmeHBwZWQ2a3d6dzQ3dmVoNzFuNyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Tg6xzp1VJ4Kl9SIa3l/giphy.gif align="center")

I will be using the most popular open-source mapping library available today — Leaflet.

We will use Leaflet to render our maps on the front end. Currently, Leaflet is available for the following web and mobile frameworks:

* [Javascript](https://leafletjs.com/)
    
* [React](https://react-leaflet.js.org/)
    
* [Vue](https://vue2-leaflet.netlify.app/)
    
* [Angular](https://github.com/bluehalo/ngx-leaflet)
    
* [Flutter](https://docs.fleaflet.dev/)
    
* [Wordpress](https://wordpress.org/plugins/leaflet-map/)
    
* [Drupal](https://www.drupal.org/project/leaflet)
    

But before we dive into Leaflet, we need to understand how digital maps work.

A digital map (such as Google Maps) is a collection of images, often called *"tiles"* — a multi-layered image grid, organized by three parameters:

1. Zoom level, indicated by Z (ranging from 0 to 22, where 0 is farthest away).
    
2. Horizontal positioning, indicated by X.
    
3. Vertical positioning, indicated by Y.
    

### Where can I get these *tiles*?

Here is the tricky part, you don't. Or at least that is what I thought until I found a way to generate them. There are map tile providers (such as MapBox) that will sell you their PNG files for a hefty price, or offer their API services.

Using their services is always an option, but they limit your creative freedom and make you dependent on a third party and an internet connection for your map application.

### The open-source map design studio, TileMill

The first tool you will need to generate your map tiles is [TileMill](https://github.com/tilemill-project/tilemill). TileMill is an open-source map design studio, using it allows you to design your maps with CSS.

Using TileMill is extremely simple! Choose an area to export (could be the entire world), choose the required zoom levels (e.g. 5 to 9) and export a `.mbtiles` file containing all of your map images. The `.mbtiles` format is used for storing map tiles in a single file.

***Fun fact:*** `.mbtiles` format uses a self-contained SQLite instance to store tile data, all within the file itself.

***Not so fun fact:*** All maps exported by TileMill are probably copyrighted to OpenStreetMap (or MapBox), I believe they can be used with proper attribution - but I am not a legal attorney and this is not legal advice! Consult with a lawyer if you wish to use TileMill maps for a commercial project (or any project).

### Got it, what’s next?

Running TileMill is easiest with docker (I love docker so much ❤️🐳). Simply clone the repository using git and run docker-compose!

```bash
git clone https://github.com/tilemill-project/tilemill.git
cd tilemill
docker-compose up
```

The first startup could take a few minutes. When it is done, navigate your browser to the following address and you should have access to TileMill.

```bash
http://localhost:20009/
```

Now that we have designed and generated our `.mbtiles` file, we will need to somehow convert it into image files — So we can serve them from a web server, or include them in our application’s binary for offline use.

To export our images we can use the tool [mbutil](https://github.com/mapbox/mbutil).

Once again it is extremely easy to set up, simply clone the repository, instantiate the Python environment and run the tool on the `.mbtiles` file exported from TileMill (for a more detailed installation guide check out mbutil's repository).

```bash
python mb-util my_map_from_tilemill.mbtiles output
```

Our output folder should have the following file structure.

```bash
0 # Zoom levels (Z)
1 - 0 # Horizontal positioning (X)
|   1 - 0.png # Final .png files, vertical positioning (Y)
|   |
n   n
```

I highly recommend passing the generated images through a tool such as TinyPNG to reduce the file size as much as possible. More tiles you export from TileMill equals more storage and/or bandwidth required by our application or server.

You will also benefit from both backend and frontend caching — since rendering a map requires a lot of tiles, which means a lot of HTTP requests.

### Serving the map

This is the section where everything comes together! I have put on my full-stack pants for this one. I am going to create a server that serves the map files and a front-end that renders them using Leaflet.

I will use Node (with Express) for the backend because Express is simple, and simple is great 😁 (the same logic applies to any language or framework).

```javascript
/* server.js */
const path = require("path");
const express = require("express");

const sendMBTilePlaceholder = async (res) => {
  const placeholderFilePath = path.join(
    __dirname,
    `./public/mbtiles/placeholder.png`
  );
  if (!res.headersSent) return res.sendFile(placeholderFilePath);
};

const sendMBTileByZXY = async ({ z, x, y }, res) => {
  //simple input validation
  if ([z, x, y].some((p) => isNaN(Number(p)))) return sendMBTilePlaceholder(res);

  const filePath = path.join(__dirname, `./public/mbtiles/${z}/${x}/${y}.png`);

  return res.sendFile(filePath, (_) => sendMBTilePlaceholder(res));
};

const main = async () => {
  const app = express();

  app.get("/mbtiles/:z/:x/:y", (req, res) => {
    const { z, x, y } = req.params;
    return sendMBTileByZXY({ z, x, y }, res);
  });

  app.listen(5000, async () => console.log(`server listening on 5000`));
};

main();
```

As you can see, I am dynamically serving PNG files from my `mbtiles` folder in the `z/x/y` format. Which is the standardized format for serving map tiles, and the format Leaflet requires.

I will now use Leaflet.js to fetch and render our map tiles on the front-end. Here is our index.html code.

```xml
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Self-hosted Map Tiles - Leaflet Demo</title>
        <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
            integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
        <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
            integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    </head>
    <body>
        <div id="map" style="height: 98vh;"></div>
    </body>
    <script>
        const lat = 35.917973;
        const lng = 14.409943;
        const initialZoom = 9;
        const map = L.map('map').setView([lat, lng], initialZoom);
    
        L.tileLayer('http://localhost:5000/mbtiles/{z}/{x}/{y}', {
            maxZoom: 9,
            minZoom: 5,
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);
    </script>
</html>
```

Over in our Javascript tag, I configured Leaflet to fetch tiles from our server, using the same `z/x/y` format. Here is our map, designed and generated using TileMill, exported using mbutil, and rendered using Leaflet.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700604404279/4eb0cf7d-a98b-4da1-8b2b-0e152c574210.png align="center")

For us to serve our map tiles offline, you just have to include them in your environment and configure your version of Leaflet to use the local file path (it varies by environment, but the concept is the same).

Now you know how to generate, serve and render maps using Leaflet, on multiple platforms and frameworks.

Happy hacking!