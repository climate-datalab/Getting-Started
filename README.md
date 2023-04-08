# Anatomy of Climate Model Output Data
----------------

This tutorial assumes that you've gotten familiar with the very basics of what a climate model is (a set of equations which describe how the climate system evolves with time). Now that you've decided that you'd like to start looking at some of the output from a model simulation, the following should help you determine how to make sense of the files you'll be looking at!


### File Format: Network Common Data Format (netCDF)
Generally speaking, most climate model output is provided in a specific file format called "netCDF", which is an abbreviation for "Network Common Data Format". More detail on netCDF files can be found [here](https://www.earthdatascience.org/courses/use-data-open-source-python/hierarchical-data-formats-hdf/intro-to-climate-data/).

__Why netCDF?__

The netCDF format may seem like a strange choice at first, but there is a method to the madness! netCDF files are what we call "self-describing", which means that within the file itself is contained all of the *metadata* needed to understand what is in the file. Think: names of variables in plain English (more on that below), information on when the file was created, model version, any transformations that have been applied to the data... all the things you might want to know if you're trying to determine how the data was generated. 

__Why are there so MANY netCDF files?__

When you start looking through some of the archives of climate model output, you'll often find that there are a LOT of files available. This happens because, well, when you're generating information for every place on Earth over hundreds of years, it ends up taking up a lot of space! To make things manageable, people usually break up the output in one of two ways (so you're not trying to read in multi-terabyte files and breaking your computer):

* Many variables for one time period 

This is sometimes called the "history file" approach by climate modeling centers, and is usually how models write out their data as they're being run. The idea is to capture a snapshot of the 'history' of the simulated climate system: so, save all of the variables (temperature, wind, ocean currents, etc etc) over a given time period. Typically, this is done by averaging all of those fields: the most common averaging period is monthly, but daily and sometimes hourly/6-hourly data is available as well.

* One variable over a longer time period

This is sometimes referred to as the "single-variable" approach, and is a very common post-processing step done by modeling centers after simulations are finished. Why? Because it's usually the type of data people actually want to use! This will vary according to your application, of course, but most environmental/climate analyses require knowing things about a GIVEN quantity as a function of time and space. For instance: you might want to see how temperature has varied through time over a given region, or to see how the statistics of precipitation have changed. 

### Model Grids and Lat/Lon Coordinates

When you open a climate model output file, you'll notice that it contains information on the latitude and longitude associated with each _grid point_. All models have some form of grid, describing how the planet has been broken up into tiles over which the relevant equations for each variable can be evaluated. There are many different types of grid, so it's good to be aware of how the data you're working with are organized!

Some things to consider:

* Is the lat/lon information ONE or TWO-dimensional?
* How far apart are adjacent grid points (in other words, what is the _resolution_ of your model?)


### Calendars Used by Climate Models

Just to make things more interesting, climate model also often use _calendars_ which are different from our "normal" conception of how to measure dates. This is usually done for reasons of making the computation easier within the model, but can cause some headaches when working with the data after the fact, since to use 'standard' time processing packages in software like R or Python, one often needs to perform a conversion. 

Some things to consider regarding climate model calendars:

* Do they include a leap year? (Many models adopt a strict 365-day calendar with no leap days)
* What is the starting date? The "time zero" for models may be quite different: often, time is measured in units of "time since xxx", where xxx is the date at which the simulation began. In order to compare with observations or another model simulation, one must therefore correctly account for this effect. 
