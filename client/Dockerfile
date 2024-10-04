# FROM node:14 AS web-build
# WORKDIR /usr/src/app
# COPY ./ ./client
# RUN cd client && npm install && npm run build --prod

# # Use official nginx image as the base image
# FROM nginx:latest

# # Copy the build output to replace the default nginx contents.
# COPY --from=web-build /usr/src/app/client/dist/client/ /usr/share/nginx/html
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# # Expose port 4200
# EXPOSE 4200

# Stage 1: Build the Angular app (using node 14)
FROM node:14 AS web-build

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy only package.json and package-lock.json to avoid re-installing dependencies if not needed
COPY ./client/package*.json ./

# Install npm dependencies
RUN npm install

# Copy the rest of the application code
COPY ./client/ ./

# Build the app
RUN npm run build --prod

# Stage 2: Serve the app with nginx
FROM nginx:latest

# Copy the built app from the previous stage into the nginx folder
COPY --from=web-build /usr/src/app/dist/client/ /usr/share/nginx/html

# Copy custom nginx configuration, if any
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 (default for nginx)
EXPOSE 4200

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
