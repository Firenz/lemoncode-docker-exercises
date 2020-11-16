FROM node:latest 
WORKDIR /usr/src/app
ENV PATH /usr/src/app/node_modules/.bin:$PATH
COPY ["package.json", "package-lock.json", "./"]
RUN npm install -g react-scripts --silent
RUN npm install --silent
COPY . .
EXPOSE 80
CMD ["npm", "start"]