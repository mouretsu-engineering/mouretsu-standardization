# ---- Base Node ----
FROM node:18-alpine AS base
WORKDIR /usr/src/app

# Copy package.json and yarn.lock for utilising Docker cache 
COPY package.json yarn.lock ./

# ---- Dependencies ----
FROM base AS dependencies  
RUN yarn install && yarn cache clean

# ---- Copy Files/Build ----
FROM dependencies AS build 
WORKDIR /usr/src/app
COPY . /usr/src/app
RUN yarn build

# --- Release ----
FROM base AS release
COPY --from=dependencies /usr/src/app/node_modules ./node_modules
COPY --from=build /usr/src/app/.next ./.next
COPY ./public ./public

# Next.js collects completely anonymous telemetry data about general usage.
# Read more here: https://nextjs.org/telemetry
# Uncomment the following line in case you want to disable telemetry.
# RUN npx next telemetry disable

EXPOSE 3000
CMD ["yarn", "start", "-H", "0.0.0.0"]