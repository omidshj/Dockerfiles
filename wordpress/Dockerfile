FROM wordpress

# Increase Upload and Memory Limit
RUN echo "file_uploads = On\n" \
         "memory_limit = 512M\n" \
         "upload_max_filesize = 128M\n" \
         "post_max_size = 128M\n" \
         "max_execution_time = 600\n" \
         > /usr/local/etc/php/conf.d/custom-limits.ini

# Increase Memory Limit for Wordpress
RUN sed \
        -i "/MySQL settings/idefine( 'WP_MEMORY_LIMIT', '256M' );" \
        /usr/src/wordpress/wp-config-sample.php

# install soap client
RUN rm /etc/apt/preferences.d/no-debian-php && \
    apt-get update && apt-get install -y libxml2-dev php-soap && \
    docker-php-ext-install soap

# Install ioncube
ADD https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz /tmp/
RUN tar xvzfC /tmp/ioncube_loaders_lin_x86-64.tar.gz /tmp/ && \
        php_ext_dir="$(php -i | grep extension_dir | head -n1 | awk '{print $3}')" && \
        mv /tmp/ioncube/ioncube_loader_lin_$(php -v | head -n 1 | cut -d " " -f 2 | cut -f1-2 -d".").so "${php_ext_dir}/" && \
    echo "zend_extension = $php_ext_dir/ioncube_loader_lin_$(php -v | head -n 1 | cut -d " " -f 2 | cut -f1-2 -d".").so" \
        > /usr/local/etc/php/conf.d/00-ioncube.ini && \
        rm /tmp/ioncube_loaders_lin_x86-64.tar.gz && \
        rm -rf /tmp/ioncube
