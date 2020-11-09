FROM node:latest AS base
WORKDIR /usr/src/app

# ---- Dependencies ----
FROM base AS dependencies
# Copy project dependencies file
COPY package*.json ./
#Install project dependencies
RUN npm install

# ---- Release ----
FROM base AS release

# copy node_modules
COPY --from=dependencies /usr/src/app/package.json ./
#Install app dependencies
#RUN npm install --silent --production && mv node_modules ../
COPY --from=dependencies /usr/src/app/node_modules ./
# copy app sources
COPY . .
# expose port
EXPOSE 80
#define CMD specify what command it'll execute when you create a container
CMD ["npm", "start"]