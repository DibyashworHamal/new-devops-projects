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

# Note: Because you are using React Router, you need to configure Nginx 
# to redirect all traffic to index.html so routes like /video/123 don't throw a 404.
# We will create a simple inline configuration for this:
RUN echo $'server { \n\
    listen 80; \n\
    location / { \n\
        root /usr/share/nginx/html; \n\
        index index.html index.htm; \n\
        try_files $uri $uri/ /index.html; \n\
    } \n\
}' > /etc/nginx/conf.d/default.conf

# Expose port 80 for Nginx
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]