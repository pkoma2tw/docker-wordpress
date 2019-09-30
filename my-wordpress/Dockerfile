FROM centos:6

RUN set -ex; \
    yum -y update; \
    yum -y install httpd php php-mbstring php-mysql php-gd php-xml wget unzip

ENV WORDPRESS_VERSION 4.9.8-ja
ENV WORDPRESS_SHA1 7680095050699294e660cf537968f12f28a21e47

RUN set -ex; \
	curl -o wordpress.tar.gz -fSL "https://ja.wordpress.org/wordpress-${WORDPRESS_VERSION}.tar.gz"; \
	echo "$WORDPRESS_SHA1 *wordpress.tar.gz" | sha1sum -c -; \
	tar -xzf wordpress.tar.gz -C /var/www/; \
  rm -rf /var/www/html/; \
  mv /var/www/wordpress/ /var/www/html/; \
	rm -f wordpress.tar.gz

# WP plugins download (zip)
WORKDIR /tmp/wp-plugins
RUN wget https://downloads.wordpress.org/plugin/custom-field-template.2.4.6.zip
RUN wget https://downloads.wordpress.org/plugin/p3-profiler.1.5.3.9.zip
#RUN wget https://downloads.wordpress.org/plugin/contact-form-7.4.6.zip
#RUN wget https://downloads.wordpress.org/plugin/syntaxhighlighter.zip
#RUN wget https://downloads.wordpress.org/plugin/wp-emmet.zip

# WP plugins install
RUN unzip './*.zip' -d /var/www/html/wp-content/plugins

# Clean up /tmp/
RUN yum clean packages
RUN rm -rf /tmp/*

# Permission
RUN chown -R apache:apache /var/www/html /var/www/html/wp-content

WORKDIR /var/www/html

# COPY docker-entrypoint.sh /usr/local/bin/
# ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["/usr/sbin/apachectl", "-DFOREGROUND"]