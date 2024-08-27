FROM node:18-alpine as build

WORKDIR /app

COPY public/ /app/public
COPY src/ /app/src
COPY package.json /app/

RUN npm install 

FROM node:18-alpine as main

COPY --from=build /app /

EXPOSE 3000

CMD ["npm", "start"]