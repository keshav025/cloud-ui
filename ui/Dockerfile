# Use a Node.js base image
FROM --platform=linux/amd64  node:14 as build-stage

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install dependencies
RUN npm install
RUN npm install axios
# Copy the rest of the application code to the container
COPY . .

# Build the Vue.js application
RUN npm run build

# Use a lightweight Node.js base image for the production environment
FROM --platform=linux/amd64  nginx:alpine as production-stage
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the built Vue.js files from the build-stage
COPY --from=build-stage /app/dist /usr/share/nginx/html
# RUN chmod +x /usr/share/nginx/entrypoint.sh
# Expose port 443
EXPOSE 80
# ENTRYPOINT ["/usr/share/nginx/entrypoint.sh"]
# Start Nginx to serve the Vue.js application
CMD ["nginx", "-g", "daemon off;"]

