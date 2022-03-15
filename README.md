# datasets-archaeo
referencing of public data sets of archaeological structures in the Arabian Peninsula and on the African continent with the addition of metadata.

### Introduction and data processing.

Here is the presentation of the processing on the raw data of the **4 public datasets** present in the repository. They each contain a latitude-longitude pair, a name and sometimes a description of the different structures located. To facilitate the processing of the data and for my needs of format for the use of these data with my future treatments by deep learning methods, I converted all the original _.kml_ files to the _**.geojson**_ format. This is one of the most widely used formats for processing geospatial data. For each point or localized shape, the format is as follows:

```json
{ "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [longitude, latitude]
        },
        "properties": {
          "name": "my_name"
        }
```
        
In each original dataset I replaced the word `name` with the word `label` to define the category of each structure.

### 1.  Sort and categorize each dataset.

For each dataset, I respected the type of structure that was listed in it. That is to say, I did not delete the structures that were not `bullseye` or `triangles`. If they had another name I did not change it either. I may have updated it, for example in the ***kengrok*** dataset, I gave the `bullseye` label to structures labeled `stone circle`. 


I attach here for all datasets the result returned by my program, which displays the list of the total number of each structure by category, the number of different categories in the dataset and finally its total number of structure.

1. *kengrok.geojson*, from the google community forum and provided by an independent user. The link to this dataset is [here](https://googleearthcommunity.proboards.com/thread/883/neolithic-sites-arabia-africa-kengrok). After sorting, it contains exactly **2679** structures listed.
> 
> ```
> [('pendant', 1129), ('bullseye', 708), ('kite', 446), ('needle', 184), ('triangle', 116), ('keyhole', 45), ('rectangle', 30), ('NA', 21)]
> the number of different categories in the dataset is: 8
> the total number of structures in the dataset is: 2679
> ```

2. *sysygy.geojson*, from the google community forum and provided by an independent user. The link to this dataset is [here](https://googleearthcommunity.proboards.com/thread/2690/ancient-saudi-stone-structures). After sorting, it contains exactly **395** structures listed.
> 
> ```
> [('bullseye', 308), ('gate', 25), ('wheel', 18), ('complex', 13), ('kite', 12), ('cairn', 8), ('wall', 5), ('NA', 2), ('pendant', 1), ('camel', 1), ('domestic', 1), ('spike', 1)]
> the number of different categories in the dataset is: 12
> the total number of structures in the dataset is: 395
> ```

It contains some very interesting structures. For example the `camel` structure is amazing, have you seen it before? The `complex` structures are also incredibly rich. The `spike` structure is intriguing. 

3. *jq_jacobs.geojson*, from the google community forum and provided by an independent user. The link to this dataset is [here](https://googleearthcommunity.proboards.com/thread/2691/saudi-stone-wheel-geoglyphs). After sorting, it contains exactly **85** structures listed.
> 
> ```
> [('bullseye', 51), ('gate', 26), ('wheel', 3), ('pendant', 2), ('needle', 2), ('wall', 1)]
> the number of different categories in the dataset is: 6
> the total number of structures in the dataset is: 85
> ```

In this dataset nothing unusual except a `wheel` structure, which seems to be a ring type without the circle at the edge with a `pendant` and a `rectangle` in the same area. It has the global_code: 7HP2J8Q9+7W if you want to observe it. I introduce the global_code in the second part, just after.

4. *bengoodspeed.geojson*, from the google community forum and provided by an independent user. The link to this dataset is [here](https://googleearthcommunity.proboards.com/thread/2691/saudi-stone-wheel-geoglyphs). After sorting, it contains exactly **2** structures listed.
> ```
> [('wheel', 2)]
> the number of different categories in the dataset is: 1
> the total number of structures in the dataset is: 2
>```


### 2.  Add metadata specific to each structure.

After sorting each dataset, I used the Google Maps API, to add to each structure the following 11 metadata:

- **country**: indicates the acronym of the country on which the structure is located (example: "SA").

- **dataset**: indicates the dataset to which the structure belongs (example: "KENGROK").

- **elevation**: indicates the altitude in meters above sea level at which the structure is located (example: 461.7255859375).

- **global_code**: is a 4-character regional code associated with a local code of 6 or more characters. This code is magic 🧙! Not only does it localize with great precision the structure to which it is associated, it transforms in a way the classical coordinates: latitude and longitude into a single code, but it is also **exclusive**. It will therefore also serve as a unique identifier associated with each structure (example: "7GQWHRRJ+W3"). Try to paste this code in Google Maps, you'll see it's a triangle !

- **index**: is the unique identifier of the structure in the _.geojson_ file (example: 9001)

- **latitude**: indicates the latitude of the located structure (example: 25.59227463255889).

- **locality**: indicates the city or municipal political entity to which the structure belongs (example: "Hadiyah").

- **longitude**: indicates the longitude of the located structure (example: 38.83014662536645).

- **place**: indicates a second-order civil entity below the country level. The equivalent of counties in the US and departments in France (example: "AlUla").

- **province**: indicates a first order civil entity below the country level. The equivalent of states in the US and regions in France (example: "Al Madinah Province").

- **resolution**: value indicating the maximum distance between data points from which the altitude has been interpolated, in meters. This property will be missing if the resolution is not known. Note that the elevation data becomes coarser (larger resolution values) when several points are passed. In this sense, the higher the resolution, the less accurate the elevation.

> Beware the metadata that informs about the administrative entities: "locality", "place" and "province"; may be missing for some structures. If the metadata is missing it means that the information was unavailable via the Google Maps API. In this case, the value associated with the key is `null`.

These metadata bring us information on each structure and will allow us to visualize and analyze in an enriched way our data and the correlations between them, if they exist. For example in the dataset of ***kengrok*** we have :

```json
{
  "geometry": {
  	"coordinates": [
  		42.84308523405771,
  		12.52564565946464
  	],
  	"type": "Point"
  },
  "properties": {
  	"country": "DJ",
  	"dataset": "KENGROK",
  	"description": "Stone circle",
  	"elevation": 232.0222473144531,
  	"global_code": "7H44GRGV+76",
  	"index": 399,
  	"label": "bullseye",
  	"latitude": 12.52564565946464,
  	"locality": "Sidiha Monghella",
  	"longitude": 42.84308523405771,
  	"place": "Alaili Dadda",
  	"province": "Obock",
  	"resolution": 152.7032318115234
  },
  "type": "Feature"
}
```

I also added `unusual` information for three structures `wheel`, `spike` and `camel` in the ***syzygy*** dataset, which are worth observing. The key returns the value `True` or does not exist.

```json
{
  "geometry": {
  	"coordinates": [
  		37.6167430172135,
  		26.87738933198257
  	],
  	"type": "Point"
  },
  "properties": {
  	"country": "SA",
  	"dataset": "SYZYGY",
  	"description": "camel",
  	"elevation": 1517.000122070312,
  	"global_code": "7GRVVJG8+XM",
  	"index": 47,
  	"label": "camel",
  	"unusual": "True",
  	"latitude": 26.87738933198257,
  	"locality": null,
  	"longitude": 37.6167430172135,
  	"place": "AlUla",
  	"province": "Al Madinah Province",
  	"resolution": 152.7032318115234
  },
  "type": "Feature"
}
```

