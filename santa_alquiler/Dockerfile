# Usar la imagen oficial de PHP con Apache
FROM php:8.2-apache

# Instalar dependencias necesarias para Laravel
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libzip-dev \
    unzip \
    git \
    libicu-dev \
    libxml2-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd zip pdo pdo_mysql bcmath

# Habilitar mod_rewrite para Apache
RUN a2enmod rewrite

# Establecer la raíz del documento de Apache a /var/www/html/public
RUN echo 'DocumentRoot /var/www/html/public' > /etc/apache2/sites-available/000-default.conf \
    && echo '<Directory /var/www/html/public>' >> /etc/apache2/sites-available/000-default.conf \
    && echo '    AllowOverride All' >> /etc/apache2/sites-available/000-default.conf \
    && echo '    Require all granted' >> /etc/apache2/sites-available/000-default.conf \
    && echo '</Directory>' >> /etc/apache2/sites-available/000-default.conf

# Copiar el código fuente del proyecto Laravel al contenedor
COPY . /var/www/html/

# Establecer el directorio de trabajo
WORKDIR /var/www/html

# Instalar Composer y dependencias de PHP
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install --no-dev --optimize-autoloader

# Establecer permisos para el almacenamiento, caché y directorio público
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache \
    && chmod -R 755 /var/www/html/public \
    && chown -R www-data:www-data /var/www/html/public

# Exponer el puerto 80 para Apache
EXPOSE 80

# Comando para iniciar Apache
CMD ["apache2-foreground"]
