FROM node:latest AS base
WORKDIR /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH

FROM base AS dependencies
WORKDIR /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH
COPY ["package.json", "package-lock.json", "./"]
RUN npm install -g react-scripts --silent
RUN npm install --silent

# FROM dependencies AS test
# COPY --from=dependencies /usr/src/app/package.json ./
# COPY --from=dependencies /usr/src/app/node_modules ./
# COPY . .
# RUN npm run test

FROM base AS release
COPY --from=dependencies /usr/src/app/package.json ./
COPY --from=dependencies /usr/src/app/node_modules ./
RUN npm install -g react-scripts --silent
COPY . ./
EXPOSE 80
CMD ["npm", "start"]