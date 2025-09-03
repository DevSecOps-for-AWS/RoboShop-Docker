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

# FROM node:20-alpine3.21
## Create a group and user ##
# RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
# WORKDIR /opt/server
#RUN chown -R roboshop:roboshop /opt/server ## this made npm install run with roboshop but package.json and server/js files are still with root ownership, moving below npm install
#USER roboshop # move it to end for application startup
# COPY package.json .
# COPY *.js .
# RUN npm install
# RUN chown -R roboshop:roboshop /opt/server
# ENV MONGO="true" \
    # MONGO_URL="mongodb://mongodb:27017/catalogue"
# USER roboshop
    # CMD ["node", "server.js"]

# above caused the image size increase from 231 Mb to 259 MB. So let's proceed with multistage build docker file#

#### Multistage Build ##############

# Stage 1: Build Environment #
# FROM  node:20-alpine3.21 AS builder
# WORKDIR /opt/server
# COPY package.json .
# COPY *.js .
# RUN npm install
# 
#Stage 2: Runtime environment #
# 
# FROM node:20-alpine3.21
# RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
# ENV MONGO="true" \
    # MONGO_URL="mongodb://mongodb:27017/catalogue"
# WORKDIR /opt/server
# USER roboshop
#COPY --from=build-stage /path/in/build/stage /path/to/place/in/final/stage #
# COPY --from=builder /opt/server /opt/server
# Define runtime configuration (e.g., CMD, ENTRYPOINT) #
# CMD ["node", "server.js"]
# 
##############################################################################################
###############################################################################################

# Stage 1: Builder
FROM node:20-alpine3.21 AS builder

# Define environment variables for user and group
ARG USERNAME=roboshop
ARG GROUPNAME=roboshop
ARG UID=1001
ARG GID=1001

# Create the group and user, then add the user to the group
RUN addgroup -g ${GID} -S ${GROUPNAME} && \
    adduser -u ${UID} -S ${USERNAME} -G ${GROUPNAME}

# Set the working directory
WORKDIR /opt/server

# Set user
USER roboshop

# Copy application files (example)
COPY package.json .
COPY *.js .

# Install dependencies as the new user
RUN chown -R ${USERNAME}:${GROUPNAME} /opt/server && \
    npm install

# Switch to the non-root user
USER ${USERNAME}

#Stage 2: Runtime environment #

FROM node:20-alpine3.21
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"
WORKDIR /opt/server
USER roboshop
#COPY --from=build-stage /path/in/build/stage /path/to/place/in/final/stage #
COPY --from=builder /opt/server /opt/server
# Define runtime configuration (e.g., CMD, ENTRYPOINT) #
CMD ["node", "server.js"]
