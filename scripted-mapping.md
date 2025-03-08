# Fun with shapes: Scripted mapping in R or Python
**When:** Saturday, March 8, from 9 a.m. - 12:30 p.m. 

**Where:** In room St. Croix I, a Mac lab on the sixth floor 

**GITHUB:** https://github.com/akanik/nicar25-scripted-mapping

Let's face it, QGIS is the Excel of geospatial analysis. Sure, doing simple mapping in it and ArcGis is a blast, but executing complex, reproducible joins and measurements can be a real drag. Taking a more scripted approach is way less of a buzzkill, especially when you need to revisit your earlier work or share with others. Whether you choose R or Python, follow along from mapping basics to more complex techniques that will make your next geospatial analysis a walk in the park. Cut loose, write some replicable code and have fun with shapes!

**Prerequisites:** To get the most out of this session, you should have a working knowledge of both GIS/mapping techniques and some experience with either Python or R.

## Apply for your Census API key
**GITHUB:** https://github.com/akanik/nicar25-scripted-mapping

APIs are an ever growing resources available to journalists. They provide a quick way to pull only the data you need from a large database. 

Many APIs are available via a base URL + filter parmeters + API key. 

The API key lets the data provider know who is accessing their information. Often times, it's also used to prevent abuse of an API.

Let's sign up for a Census API key so we can quickly, easily and programmatically pull only the varibles we will need for our session today.

https://api.census.gov/data/key_signup.html

NOTE: You should receive your key within 10 minutes. If you DON'T receive your key in that time, email census.data@census.gov from the email address you want to associate with your key. Include the following message:

```
Hello, I have attempted to sign up for a Census API key but have not received a response. Jessica Barnett said to email this account if I was having issues.

[Your name]
[Your orgnaization]
[Your email address]

Thank you for your help!
```

## Let's get this party started
**GITHUB:** https://github.com/akanik/nicar25-scripted-mapping

**CONF COMPUTERS:** Desktop/....

We're going to walk you through some common but crucial scripted mapping concepts that will likely get you most of the way through many of the geospacial projects you'll be working on. 

### For R users: 
R doesn't use virtual environments so y'all are lucky ducks! You can use either VSCode or RStudio for this class. Use your preferred application to open the R example code file: [code/r-mapping.ipynb](code/r-mapping.ipynb)

### For Python users:
We recommend you use VSCode so that the virtual environment behaves. And honestly I forgot to install `jupyterlab` so you'll have to play by my rules today. 

- Open a new VSCode window
- Click File > Open folder
- Navigate to where you cloned this repo OR the class folder
- Open the example code file: [code/python-mapping.ipynb](code/python-mapping.ipynb)
- Click on the "Select Kernal" button in the top right of your VSCode screen
- Click Python environments
- The first option with a start by it should be `env/bin/python`
- Try and run the first cell in [the example code file](code/python-mapping.ipynb). If it runs without error, you're solid. If you get errors, raise yo hand there's an issue with your virtual env

### For everyone
You can now follow along in your chose language example file. We've included code comments to help you understand specifically what each snippet of code does. 

We're also including notes in this doc that will help you understand the importance of mapping concepts we'll be covering today. 

## Data sourcing and useful resources

Helpful Minneapolis codes:
- Minnesota is state fips 27
- Minneapolis is in Hennepin County, which is county fips 27053.

Where to download/find these GIS files for your state!
- **Schools:** [The NCES EDGE open data portal](https://data-nces.opendata.arcgis.com/) contains all sorts of helpful data, including school locations.
- **Neighborhoods:** check your city GIS site
- **Toxic Release Inventory:** Multiple years and states [available here](https://www.epa.gov/toxics-release-inventory-tri-program/tri-basic-data-files-calendar-years-1987-present)
- **Census Tigerline URLs:** [pick your vintage here](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html), then click FTP Archive and navigate to the shapefile you need.

## Projections
We all only make the medium bucks here, so we're not going to get into the nitty gritty of the math behind projections and reprojections or why one might be the best option over the other. The key concepts to remember here are that we're taking a 3D globe and trying to cut it down to size to make a flat, 2D map! And with that comes some complicated map math.

With most spatial data analysis, you'll typically run into a few industry favorites for a coordinate reference system, or CRS. [EPSG.io](https://epsg.io) is a great resource for searching through EPSG codes, which are a sort of index code assigned to different ways to visualize that 3D to 2D transition (or vice versa). 

In order to calculate distance on the ground in a unit that doesn't make you want to tear your hair out (like in meters or feet or miles), you need to reproject your layer into one that supports **meters** and not **degrees**. We're picking `EPSG:6505`, which is also known as `NAD83`. Other popular ones are `EPSG:4326`, or `WGS84`, as well as `EPSG:3857`, aka `WGS84/Pseudo-Mercator`, which is what most web maps use.

## Slaying with APIs 

Many ArcGIS maps have a download button that allows you to directly download map data onto your computer. But this is a super manual step in an otherwise scripted process, right? 

Never fear! A lot of ArcGIS maps have helpful API portals that will walk you thru querying and downloading the geojson. Let's walk through that now.

### Finding and downloading underlying data in online ArcGIS maps
Have you ever found a map with really interesting data but there's no download button? If it's an ArcGIS map you may be in luck.

Let's take this City of Houston map: https://www.arcgis.com/apps/mapviewer/index.html?webmap=c1eae6e9a0bb48cf8000e960780b752a

This apparently has a ton of data but I can't see a way to download any of it. Enter Browser Network tab. 

Here's a video on how to do this: https://youtu.be/bqSBfOTTGwU

### The safe way to import an API key
Your API key should ALWAYS be private. So what about when you want to push your code to github to share with coworkers or the world? For this we use environment variables.

Let's set those up now.

**Python users:**

- In VSCode, add a new file called `.env`
- Open the file and add the following line `census_api_key=[add_your_api_key_here_please]`
- Save the file and close
- Make sure `.env` is in your `.gitignore` file
- At this point, you may need to close your notebook and reopen to reactivate the virtual environment so it sees the new variable we just loaded.


## Crosswalks: When to use which geometry level
Sometimes, you have two different geographic boundaries that you want to link togeher in order to make conclusions about them both. (An example of this would be school district boundaries and cities, or Census tracts and neighborhoods, which we'll use with our coding demo!)

The caveat: you want to always make sure you're weighing your data by the area it overlaps. Otherwise, you might be making improper conclusions. Smaller geographies can help with this, but always confirm it with on the ground knowledge as well!

## Buffers: Finding experts to inform your geographic decision-making
Buffers can be incredibly useful and powerful tools to quickly determine proximity of two things to each other. They're quick, they're easy, and relatively painless, especially once you have the correct CRS in place!

One thing to be aware of here are predicates, which are essentially different Boolean tests run against different shapes. There's a lot to get into there [and we wrote about it for our class on this last year in more detail](https://github.com/akanik/nicar24-mapping-session/blob/main/qgis-walkthrus/qgis-spatial-joins.md#executing-spatial-joins) with pictures and examples.

We can (and will) use these concepts to help us navigate which predicates to select for our own data. And sticking with the theme of framing things as questions, this is how I think about predicates for myself:
* intersect — "`does layer one share any space with layer two?`"
* contain — "`is layer two completely overlapped by layer one?`"
* equal = "`does layer one match layer two?`"
* touch = "`do layer one and layer two touch at all?`"
* overlap = "`do layer one and layer two share some space, but not all?`"
* are within = "`is layer one completely overlapped by layer two?`"
* cross = "`do i really genuinely like making maps?`" (kidding — "`is layer two a line, and does it intersect or touch both ends of layer one?`")

These aren't perfect by any means, but they help guide some of my decision-making. I wish I had a cool mnemonic device to share, but the truth is that with spatial data analysis, and data journalism as a whole, you don't need to have the right answers memorized — you just need to know where to find them. :) 

