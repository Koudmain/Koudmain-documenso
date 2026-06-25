# FROM node:22-alpine AS development-dependencies-env
# COPY . /app
# WORKDIR /app
# RUN npm install

# FROM node:22-alpine AS production-dependencies-env
# COPY ./package.json package-lock.json /app/
# WORKDIR /app
# RUN npm install --omit=dev

# FROM node:22-alpine AS build-env
# COPY . /app/
# COPY --from=development-dependencies-env /app/node_modules /app/node_modules
# WORKDIR /app
# RUN apk add --no-cache bash
# RUN npm run build

# FROM node:22-alpine
# WORKDIR /app
# COPY --from=build-env /app/apps/remix/package.json /app/package.json
# COPY --from=build-env /app/apps/remix/build /app/build
# COPY --from=production-dependencies-env /app/node_modules /app/node_modules
# CMD ["npm", "run", "start"]

FROM node:22-alpine

ARG LOCAL_IP

ENV LOCAL_IP=${LOCAL_IP}

RUN apk add --no-cache bash

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build --workspace=@documenso/prisma

RUN npm run translate:compile

EXPOSE 3000

CMD ["npm", "run", "dev", "--workspace=@documenso/remix", "--", "--host"]
