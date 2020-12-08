

CHEATER???!!!
==========

If you read the rest of this file then you're a cheater! Booooh!


I am a cheater
=======


## The application
### Random ports solutions

Add the following juste before `shinyApp(...)` at the end of app.R:
```
options(shiny.host="0.0.0.0")
options(shiny.port=8765)
```

## The Dockerfile

### Base image
We'll use the following base image: workflow4metabolomics/gie-shiny
```
FROM quay.io/workflow4metabolomics/gie-shiny:latest
```


### System Packages
You must install the following packages to your docker image: r-cran-car liblzma-dev libbz2-dev .
This translates to:
```
RUN apt-get update									\
	&& apt-get install --no-install-recommends -y 	\
      	r-cran-car									\
      	liblzma-dev									\
      	libbz2-dev									;
```


### Install R packages
There is a script that install these packages to the good version!
```
COPY ./packages.R /tmp/packages.R
RUN Rscript /tmp/packages.R
```

### Add the app to the docker image
Let's create a directory that will contain our application
```
RUN 	mkdir  /heatmap				\
	&&	chmod -R 755 /heatmap		\
	&&	chown shiny.shiny /heatmap	;
```

And then, add necessary files to this directory:
```
COPY ./app.R /heatmap/app.R
COPY ./static/css/styles.css /heatmap/styles.css
```