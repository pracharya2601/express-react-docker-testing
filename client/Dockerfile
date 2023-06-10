
FROM node:16-alpine AS deps
WORKDIR /app
COPY client/package.json /app
RUN yarn install

FROM node:16-alpine as build
WORKDIR /app
COPY --from=deps /app/node_modules node_modules
COPY . /app
RUN yarn run build

FROM node:16-alpine AS runner
WORKDIR /app
COPY --from=build /app/build build

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 cra

ENV NODE_ENV production
EXPOSE 5002
CMD [ "npx", "serve", "build" ]