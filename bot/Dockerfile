FROM node:20-bullseye

RUN apt-get update && apt-get install -y git curl

WORKDIR /app

# Clone repo
ARG GIT_REPO=https://github.com/Dragory/ZeppelinBot.git
ARG GIT_BRANCH=master
RUN git clone $GIT_REPO . && git checkout $GIT_BRANCH

# Install dependencies (workspace aware)
RUN npm install

# Build entire project
RUN npm run build

# Set env vars
ARG DB_DATABASE
ARG DB_PASSWORD
ARG DB_PORT
ARG DB_USER
ARG DB_HOST
ARG API_URL
ARG DASHBOARD_URL
ARG KEY
ARG STAFF
ARG CLIENT_SECRET
ARG CLIENT_ID
ARG BOT_TOKEN
ARG RAILWAY_SERVICE_API_URL

# Wait for API to be available
RUN echo "Waiting for API Url $API_URL to be available.."
RUN timeout 300 bash -c 'while [[ "$(curl -s -o /dev/null -w ''%{http_code}'' $API_URL)" != "200" ]]; do sleep 5; done' || false

CMD ["npm", "run", "start-bot-prod"]
