# Script Deploy NodeJS App in Google Cloud App Engine & Cloud Run
Step before deploy app engine and cloud run

 1. Create Project
 2. Enable Cloud Build API  ([Click Here to Enable Cloud Build API](https://console.cloud.google.com/flows/enableapi?apiid=cloudbuild.googleapis.com&_gl=1*g63n42*_up*MQ..&gclid=CjwKCAjwrIixBhBbEiwACEqDJa-yPE5BuXV8mColonABOwtjjeIJ51Ev1xMm31PKFSfs1rlRoUeF5xoC9kQQAvD_BwE&gclsrc=aw.ds&_ga=2.111412806.312993319.1713533051-2142620159.1712142868&_gac=1.195686110.1713567852.CjwKCAjwrIixBhBbEiwACEqDJa-yPE5BuXV8mColonABOwtjjeIJ51Ev1xMm31PKFSfs1rlRoUeF5xoC9kQQAvD_BwE))

## App Engine

 1. Enable App Engine API [Click Here](https://console.cloud.google.com/apis/library/appengine.googleapis.com?project=cc-03-belajar-deployy)
 2. Clone your repo in google cloud shell
 3. Prepare database
 4. Create app.yaml file and fill the file with this code
	 ```
	runtime: nodejs20
	```
 5. Make sure your package.json script is this
	```
	"scripts": {  
		"start": "node server.js"
	}
	```
 6. Run this command to deploy to App Engine
	 ```
	gcloud app deploy
	```
 8. Test your app

## Cloud Run

 1. Clone your repo in google cloud shell
 2. Prepare database
 3. Create artifact registry repo
 4. Create Dockerfile file and fill the with this code
	 ```
	 #Use the official Node.js 14 image as the base image
	FROM node:20

	#Set the working directory in the container
	WORKDIR /usr/src/app

	#Copy package.json and package-lock.json to the working directory
	COPY package*.json ./

	#Install app dependencies
	RUN npm install

	#Copy the rest of the application code to the working directory
	COPY . .

	#Expose the port your app runs on
	EXPOSE 8080

	#Command to run your application
	CMD ["npm", "start"]
	 ```
 5. Build docker image and don't forget to tag it
	 ```
	 docker build -t <image-name>:<tag> .
	 ```
 6. Create tag to Artifact Registry
	 ```
	 docker tag <image_name>:<tag> <location>-docker.pkg.dev/<project_id>/<repository_name>/<image_name>:<tag>
	 ```
 7. Push docker image to artifact registry
	```
	docker push <location>-docker.pkg.dev/<project_id>/<repository_name>/<image_name>:<tag>
	```
 8. Deploy cloud run by pull docker image from artifact registry
	 ```
	 gcloud run deploy <service-name> \ 
		 --image <image-uri> \ 
		 --region <region> \ 
		 --platform managed
	 ```
 9. Test your app
