FROM node:4

EXPOSE 5000

COPY app /app
WORKDIR /app

RUN npm install 
RUN npm install -g pm2

CMD pm2 start index.js -i max --no-daemon
