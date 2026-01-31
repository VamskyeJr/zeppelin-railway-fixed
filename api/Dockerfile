FROM node:20-bullseye

RUN apt-get update && apt-get install -y git

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
ARG DASHBOARD_URL
ARG API_URL
ARG KEY
ARG STAFF
ARG CLIENT_SECRET
ARG CLIENT_ID
ARG BOT_TOKEN
ARG DEFAULT_ALLOWED_SERVERS
ARG API_PORT=3000

# Run migrations
RUN npm run migrate-prod

RUN echo "API Ready. Your discord oauth redirect url is: $API_URL/auth/oauth-callback"

EXPOSE 3000

CMD ["npm", "run", "start-api-prod"]
