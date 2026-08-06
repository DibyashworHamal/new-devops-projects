FROM node:26-alpine as build

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci

# Copy the rest of the source code
COPY . .

# Build the Vite project (this outputs static files to a 'dist' folder)
RUN npm run build


FROM nginx:alpine

# Copy the built files from Stage 1 into Nginx's HTML serving folder
COPY --from=build /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 for Nginx
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]