# Police Map

> A Vue.js project using the UK police API

## Build Setup

``` bash
# install dependencies
node
npm
npm install

# serve with hot reload at localhost:1337
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation about the [task](https://github.com/syzygy-uk/crime-map)

## Potential Additions
* More stlyed map and marker icons relevant to crime type
* More advanced control panel
* Crime heat map layer.
* Polygon drawing to find crime in a drawn area
* Enter a postcode and resolve to a lat lng / geolocation

## Issues
* The objective was to find crime in the last month, unfortunatly the last month returns no results. So i have hard coded the date, you are able to uncomment the line and it will use a dynamic date.
