# GEOIMAGE
#### A Javascript library for generating bitmaps out of **geoTIFF** files.
<img src = "/images/example0crop1.png" width = "100%">

Contains a GeoImageOptions data type and GeoImage class

## Features
#### Color texture generation
- Create RGB pictures out of RGB geoTIFF data.
- Generate pictures out of non-RGB geoTIFF data with different processing options.

#### Terrain texture generation
- Generate heightmaps out of single-channel geoTIFF elevation data.
- The **elevation data** is encoded into the bitmap as [Mapbox Terrain-RGB](https://docs.mapbox.com/data/tilesets/guides/access-elevation-data/#decode-data).

#### Data visualisation options
- Color
- Transparency
- Heatmap
- Data slice
- Automatic data range
- Manual data range
## GeoImageOptions

- `type : 'image' | 'terrain'`- see [return options](###Return-options) for more details

- `format: 'UINT8' | 'UINT16' | 'UINT32' | 'FLOAT32' | 'FLOAT64'` - choose input format

- `useAutoRange : boolean` - set automatic range of color gradient **(default false)**
- `rangeMin : number | null` - set minimal value range **if useAutoRange is false**  **(default 0)**
- `rangeMax : number | null` - set maximal value range **if useAutoRange is false**  **(default 255)**
- `useDataForOpacity : boolean` - visualise data with opacity of each pixel according to its value **(default false)**
- `alpha : number` - visualise data in specific opacity **(if useDataOpacity is false)** **(default 150)**
- `useHeatMap : boolean` - generate data as a color heatmap **(default true)**
- `color [number, number, number]` - generate data in specific color **(if useHeatMap is false)**
- `useChannel : number | null` - specify a single channel to use **(default null)**
- `multiplier : number  ` - multiplies each value **(default 1.00)**
- `clipLow : number | null`- generate only data greater than this **(default null)**

- `clipHigh : number | null`- generate only data less than this **(default null)**

## Return options
**Method returns Image DataUrl**

- `getMap( input : string | { width : number, height : number, rasters : any[] }, options: GeoImageOptions)`

  If `options.type` = `"image"` - If the input has 3 or 4 color channels, return standard RGB or RGBA image. If the input has 1 channel, data gets processed according to data processing options.

  If `options.type` = `"terrain"` - Ignores all options except `multiplier` and returns  [Mapbox Terrain-RGB](https://docs.mapbox.com/data/tilesets/guides/access-elevation-data/#decode-data)

## Basic example
#### Initialize the library
```
import GeoImage from 'geoimage';
import GeoImageOptions from 'geoimage';

const g = new GeoImage();
```


#### Get bitmap



##### Create options

```
options : GeoImageOptions = { 
type:"image",
format:"UINT8",
multiplier:1.0,
useChannel:1,
alpha:180,
useAutoRange: true
}
```



##### Call method

```
const bitmap = await g.getMap({
		width : 512,
    	height : 512,
    	rasters : [[...data]]
    },{
	type : 'image',
    format : FLOAT32
    } );
```
```
const bitmap = await g.getMap(
	input : {
		width : 512,
    	height : 512,
    	rasters : [[...data]]
    	},
    options
    );
```



#### Get heightmap



##### Create options

```
options : GeoImageOptions = { 
type:"terrain",
format:"UINT8",
multiplier:1.0,
useChannel:1,
alpha:180,
useAutoRange: true
}
```



##### Call method

```
const bitmap = await g.getMap({
		width : 512,
    	height : 512,
    	rasters : [[...data]]
    },{
		type : 'terrain',
    	format : FLOAT32
    });
```
```
const bitmap = await g.getMap(
	input : {
		width : 512,
    	height : 512,
    	rasters : [[...data]]
    	},
    options
    );
```

