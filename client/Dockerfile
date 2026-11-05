# Use a Node.js base image for building
FROM --platform=linux/amd64 node:26.12.1 AS builder

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY ./package.json ./package-lock.json ./.npmrc ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Pass build-time environment variables (only available during build stage)
ARG REACT_APP_API_URL
ENV REACT_APP_API_URL=$REACT_APP_API_URL

# Build the frontend app
RUN npm run build

# Use Nginx to serve the frontend build
FROM --platform=linux/amd64 nginx:1.27-alpine
COPY --from=builder /app/build /var/www/app
COPY ./infra/nginx/conf/ /etc/nginx/conf.d
EXPOSE 80 443
