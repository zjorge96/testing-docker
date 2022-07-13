FROM alpine:latest

WORKDIR /backend
# 172.17.0.2
ENV MONGODB_URI=mongodb://172.17.0.2:27017/
ENV NODE_ENV=production
ENV SECRET=secret
EXPOSE 3030

RUN apk add git
RUN apk add --update nodejs npm

RUN git clone https://github.com/gothinkster/node-express-realworld-example-app.git

# Install everything needed (--prefix to install in target directory)
RUN npm i --prefix ./node-express-realworld-example-app

# Start the backend
CMD npm start --prefix ./node-express-realworld-example-app
# CMD ["sleep", "infinity"]