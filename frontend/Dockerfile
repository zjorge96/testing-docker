FROM alpine:latest

WORKDIR /frontend
EXPOSE 4100

# Dependencies
RUN apk add git
RUN apk add --update nodejs npm

# Install the application
RUN git clone https://github.com/gothinkster/react-redux-realworld-example-app.git

# Alter backend
RUN sed -i "s/https:\/\/conduit.productionready.io/http:\/\/localhost:3030/" ./react-redux-realworld-example-app/src/agent.js

# Install everything needed
RUN npm i --prefix ./react-redux-realworld-example-app

# Start the frontend
CMD npm start --prefix ./react-redux-realworld-example-app