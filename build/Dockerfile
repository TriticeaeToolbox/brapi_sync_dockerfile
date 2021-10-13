#
# BUILDER - build the angular project
#

FROM node:12-slim AS builder

# Install Build Dependencies
RUN apt update
RUN apt install -y git

# Install BrAPI-Sync project
ARG brapi_sync_repo
ARG brapi_sync_branch
RUN git clone --depth 1 --branch ${brapi_sync_branch} https://github.com/${brapi_sync_repo} /brapi_sync

# Build the project
WORKDIR /brapi_sync
RUN npm install
RUN npm install https://github.com/solgenomics/BrAPI.js
RUN npm --prefix=/brapi_sync/node_modules/\@solgenomics/brapijs/ run prepare
RUN npm run build


#
# PRODUCTION - serve the built project
#

FROM nginx:stable AS production

# Copy the nginx config from host
COPY --chown=nginx:nginx ./conf.d /etc/nginx/conf.d

# Copy the project dist directory from builder step
RUN mkdir /usr/share/nginx/brapi_sync && chown nginx:nginx /usr/share/nginx/brapi_sync
COPY --from=builder --chown=nginx:nginx /brapi_sync/dist/brapi-sync-angular /usr/share/nginx/brapi_sync