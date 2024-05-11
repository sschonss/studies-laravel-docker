# Use the official PHP 8.2 image
FROM php:8.2

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libpq-dev \
    libzip-dev \
    libonig-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql pdo_pgsql mbstring zip exif pcntl bcmath gd

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Node.js and npm (for Laravel Mix)
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs

# Install MongoDB extension dependencies
RUN apt-get update && apt-get install -y \
    libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Install MongoDB extension
RUN pecl install mongodb && docker-php-ext-enable mongodb

# Install Redis extension
RUN pecl install redis && docker-php-ext-enable redis

# Set the working directory
WORKDIR /var/www/app

# Copy the application files
COPY . /var/www/app

# Set permissions for Laravel
RUN chown -R www-data:www-data /var/www/app
RUN chmod -R 755 /var/www/app/storage

# Expose ports
EXPOSE 80

# Start PHP built-in server
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=80"]
