FROM node:11.13.0-alpine

ENV APP_ROOT /usr/src/spface/ui

# RUN mkdir ${APP_ROOT}
WORKDIR ${APP_ROOT}
ADD . ${APP_ROOT}

RUN npm install
RUN npm run build

ENV HOST 0.0.0.0