##### Non optimized image with size was 1.25GB ###### 
# FROM node:20
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV MONGO="true" \
    # MONGO_URL="mongodb://mongodb:27017/catalogue"
# CMD ["node", "server.js"]

#### Catalogue docker image Optimization - optimized to 231 MB ####

# FROM node:20-alpine3.21
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV MONGO="true" \
    # MONGO_URL="mongodb://mongodb:27017/catalogue"
# CMD ["node", "server.js"]

#### Catalogue docker image Optimization - Further optimization from  231 MB ####

FROM node:20-alpine3.21
## Create a group and user ##
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
WORKDIR /opt/server
#RUN chown -R roboshop:roboshop /opt/server ## this made npm install run with roboshop but package.json and server/js files are still with root ownership, moving below npm install
#USER roboshop # move it to end for application startup
COPY package.json .
COPY *.js .
RUN npm install
RUN chown -R roboshop:roboshop /opt/server
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
USER roboshop
    CMD ["node", "server.js"]