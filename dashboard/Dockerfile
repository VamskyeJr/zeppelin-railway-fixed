FROM node:20-bullseye

RUN apt-get update && apt-get install -y git nginx openssl

ARG API_URL
ARG DOMAIN_NAME
ARG NODE_ENV
ARG API_PROXY_FQDN

# Generate self-signed SSL cert
RUN openssl req -x509 -newkey rsa:4096 -keyout /etc/ssl/private/zeppelin-self-signed-cert.key -out /etc/ssl/certs/zeppelin-self-signed-cert.pem -days 3650 -subj "/CN=${DOMAIN_NAME}" -nodes

WORKDIR /etc/nginx/sites-available

COPY zeppelin .
RUN sed -ir "s/_API_PROXY_FQDN_/${API_PROXY_FQDN}/g" zeppelin
RUN sed -ir "s/_DOCKER_PROD_DOMAIN_/${DOMAIN_NAME}/g" zeppelin
RUN rm -f /etc/nginx/conf.d/default.conf
RUN ln -s /etc/nginx/sites-available/zeppelin /etc/nginx/conf.d/default.conf

WORKDIR /app

# Clone repo
ARG GIT_REPO=https://github.com/Dragory/ZeppelinBot.git
ARG GIT_BRANCH=master
RUN git clone $GIT_REPO . && git checkout $GIT_BRANCH

# Create .env file
RUN touch .env
RUN echo "API_URL=$API_URL" > .env

# Install dependencies (workspace aware)
RUN npm install

# Build entire project
RUN npm run build

# Build dashboard with webpack
RUN cd dashboard && npx cross-env NODE_ENV=web webpack --config webpack.config.js

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
