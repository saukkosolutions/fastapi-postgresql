# Stage 0, "build-stage", based on Node.js, to build and compile the frontend
FROM greenaj/node-frontend:18 as build-stage

WORKDIR /app

COPY package*.json /app/

RUN npm install

COPY ./ /app/

ARG FRONTEND_ENV=production
ARG NODE_OPTIONS=--openssl-legacy-provider

ENV VUE_APP_ENV=${FRONTEND_ENV}

# Comment out the next line to disable tests
# RUN npm run test:unit

RUN npm run build


# # Stage 1, based on Nginx, to have only the compiled app, ready for production with Nginx
FROM nginx:1.22.1

COPY --from=build-stage /app/dist/ /usr/share/nginx/html

COPY --from=build-stage /default.conf /etc/nginx/conf.d/default.conf
COPY ./nginx-backend-not-found.conf /etc/nginx/extra-conf.d/backend-not-found.conf
