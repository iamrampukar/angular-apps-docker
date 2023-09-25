## Step 1
##  Dockerfile
FROM node:14.20.0-alpine as build-step
RUN mkdir -p /app
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
RUN npm run build --prod
## Step 2
FROM nginx:1.20.1
COPY --from=build-step /app/dist/angular-apps /usr/share/nginx/html
EXPOSE 4200:80

## docker-compose.yml
version: "3.7"
services:
    angular-service:
       container_name: ng-docker-example
       build: .
       ports:
           - "4200:80"
		   
## The build command should look as shown below
	docker-compose build angular-service
	docker-compose up
	http://localhost:4200
## Source:
https://www.section.io/engineering-education/containerizing-an-angular-app-featuring-nginx-web-server-using-docker/