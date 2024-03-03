# Use the Nginx image from Docker Hub
FROM nginx

# Remove the default Nginx configuration file
RUN rm /etc/nginx/conf.d/default.conf

# Copy a new configuration file from the current directory
COPY nginx.conf /etc/nginx/conf.d/default.conf


# Create and set content for index.html
RUN echo "Welcome to nginx!" > /usr/share/nginx/html/index.html

# Create and set content for healthy.html
RUN echo "healthy" > /usr/share/nginx/html/healthy.html

# Expose port 3333
EXPOSE 3333
