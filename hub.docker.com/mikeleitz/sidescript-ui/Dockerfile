# build stage
FROM node:lts-alpine as build-stage
MAINTAINER leitz@mikeleitz.com

WORKDIR /app
COPY package*.json ./
# For some reason, vue cli doesn't work here.  Have to install it manually.
RUN npm i @vue/cli-service
RUN npm install --production
COPY . .
RUN npm run build

# production stage
FROM nginx:stable-alpine as production-stage
COPY --from=build-stage /app/target/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
