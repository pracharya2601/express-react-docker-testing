FROM node:16-alpine AS deps
WORKDIR /app
COPY ./package.json /app
RUN yarn install

FROM node:16-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules node_modules
COPY . /app
RUN yarn run build

FROM node:16-alpine AS runner
WORKDIR /app
COPY --from=build /app/node_modules node_modules
COPY --from=build /app/dist dist

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 expressjs

USER expressjs
EXPOSE 5001
ENV PORT 5001
CMD ["node", "dist/index.js"]
