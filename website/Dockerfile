# Use the official PHP Apache image as a base
FROM php:8.2-apache

# Enable mod_rewrite (commonly used in PHP applications)
RUN a2enmod rewrite

COPY website/ /var/www/html/

# Set proper permissions for the files
RUN chown -R www-data:www-data /var/www/html

# Expose port 80 to access the web server
EXPOSE 80

# Start the Apache server
CMD ["apache2-foreground"]