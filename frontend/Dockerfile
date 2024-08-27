FROM node:18-alpine as module-install-build-stage

WORKDIR /app

COPY public/ /app/public
COPY src/ /app/src
COPY package.json /app/

RUN npm install 

RUN npm run build

FROM node:18-alpine

COPY --from=module-install-build-stage /app/build /app/build
RUN npm install -g serve

EXPOSE 3000

# CMD ["npm", "start"]

CMD ["serve", "-s", "/app/build"]