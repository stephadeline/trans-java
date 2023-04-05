# Cartography module Final Project: Impact of Trans-Java Toll Road

This is Stephanie Adeline's final project for the Cartography module of MVTEC.

The online link is here, deployed via vercel: [https://trans-java.vercel.app/](https://trans-java.vercel.app/)

If you wish to run this project locally, you can clone it and run:
`npm i` and `npm run dev`.

This story is Stephanie’s final project for the cartography module in the Master’s in Visual Tools to Empower Citizens program from the University of Girona and the Visualization for Transparency Foundation.

The text in this project has not been fact-checked or edited. Please take it as a proof of concept only.

## Methodology
The spatial data for the toll roads in operation and under construction, along with data on rest areas and toll gates were obtained from data.go.id.

The shapefile boundaries for provinces were obtained from [Indonesia Geospasial](https://www.indonesia-geospasial.com/2020/04/download-shapefile-shp-batas.html).

Data for when toll roads started operating was obtained from [Badan Pengatur Jalan Tol](https://bpjt.pu.go.id/konten/progress/jalan-tol-ppjt). This data is then joined in QGIS with the toll-road lines, and manual checks were done to ensure the names joined were perfectly matching.

Satellite imagery was obtained from Landsat 8 via EO browser. Flood raster image was also obtained from [EO browser](https://apps.sentinel-hub.com/eo-browser/) using Sentinel 1 data.

Further satellite image timelapses were obtained from Google Earth Pro.

This page was built using Svelte and maps were built using MapLibre. The style is from MapTiler.

The content for the text was created in a [Google Sheet](https://docs.google.com/spreadsheets/d/1aHVn_qIDBXCEVmpZCjRrOSiN5d58kjkRxQ4x2t_nChU/edit?usp=sharing) which was then converted to Json and stored in the [data folder](https://github.com/stephadeline/trans-java/tree/main/src/data), along with other geojson files.

## Challenges, potential improbements, and some work that I did but didn't end up including

I looked into the NDVI of the areas, using a buffer of the toll road but didn't find anything meaningful. See this [Google Earth Script](https://code.earthengine.google.com/85b2e5ced160193150b4eb11200a5c54) for the script I used.

I also wanted to add tool tips for the routes and toll gates, but only had time to do for rest-areas.

---

# svelte app

This is a project template for [Svelte](https://svelte.dev) apps. It lives at https://github.com/sveltejs/template.

To create a new project based on this template using [degit](https://github.com/Rich-Harris/degit):

```bash
npx degit sveltejs/template svelte-app
cd svelte-app
```

*Note that you will need to have [Node.js](https://nodejs.org) installed.*


## Get started

Install the dependencies...

```bash
cd svelte-app
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:8080](http://localhost:8080). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.

If you're using [Visual Studio Code](https://code.visualstudio.com/) we recommend installing the official extension [Svelte for VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode). If you are using other editors you may need to install a plugin in order to get syntax highlighting and intellisense.

## Building and running in production mode

To create an optimised version of the app:

```bash
npm run build
```

You can run the newly built app with `npm run start`. This uses [sirv](https://github.com/lukeed/sirv), which is included in your package.json's `dependencies` so that the app will work when you deploy to platforms like [Heroku](https://heroku.com).


## Single-page app mode

By default, sirv will only respond to requests that match files in `public`. This is to maximise compatibility with static fileservers, allowing you to deploy your app anywhere.

If you're building a single-page app (SPA) with multiple routes, sirv needs to be able to respond to requests for *any* path. You can make it so by editing the `"start"` command in package.json:

```js
"start": "sirv public --single"
```

## Using TypeScript

This template comes with a script to set up a TypeScript development environment, you can run it immediately after cloning the template with:

```bash
node scripts/setupTypeScript.js
```

Or remove the script via:

```bash
rm scripts/setupTypeScript.js
```

If you want to use `baseUrl` or `path` aliases within your `tsconfig`, you need to set up `@rollup/plugin-alias` to tell Rollup to resolve the aliases. For more info, see [this StackOverflow question](https://stackoverflow.com/questions/63427935/setup-tsconfig-path-in-svelte).

## Deploying to the web

### With [Vercel](https://vercel.com)

Install `vercel` if you haven't already:

```bash
npm install -g vercel
```

Then, from within your project folder:

```bash
cd public
vercel deploy --name my-project
```

### With [surge](https://surge.sh/)

Install `surge` if you haven't already:

```bash
npm install -g surge
```

Then, from within your project folder:

```bash
npm run build
surge public my-project.surge.sh
```
