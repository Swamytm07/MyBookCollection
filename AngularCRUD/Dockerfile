
FROM node:16.14.0-alpine AS build

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM nginx:alpine

COPY --from=build /app/dist/angular-crud/ /usr/share/nginx/html/

EXPOSE 80
