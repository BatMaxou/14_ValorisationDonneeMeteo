#Build
FROM dhi.io/node:24-dev AS build-stage

ENV NODE_ENV=production
WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# Run
FROM dhi.io/node:24 AS runtime-stage

ENV NODE_ENV=production
WORKDIR /usr/src/app

COPY --from=build-stage /usr/src/app/node_modules ./node_modules
COPY --from=build-stage /usr/src/app/.nuxt ./.nuxt
COPY --from=build-stage /usr/src/app/.output ./.output
COPY --from=build-stage /usr/src/app/.env ./.env

USER 1000

EXPOSE 3000

CMD [ "node", ".output/server/index.mjs" ]
