
FROM node:10-alpine

LABEL com.becorecode.author="Alfonso Alba García"
LABEL com.becorecode.author_email="hola@becorecode.com"

ENV APP_PATH=/home/node
ENV APP_PORT=8000
ENV NODE_NO_WARNINGS=-1
USER node

WORKDIR $APP_PATH

COPY --chown=node . $APP_PATH

RUN npm install && npm audit fix

EXPOSE $APP_PORT

CMD ["npm", "start", "--", "--port=${APP_PORT}"]
