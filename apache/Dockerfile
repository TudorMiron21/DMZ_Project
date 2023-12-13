# Use a specific version of the httpd image from Docker Hub
FROM httpd:2.4

# Copy your custom Apache configuration file into the container
# This line is optional if you're using the default configuration
# COPY ./my-httpd.conf /usr/local/apache2/conf/httpd.conf

# Copy the self-signed certificate and private key into the container
COPY ./server.crt /usr/local/apache2/conf/server.crt
COPY ./server.key /usr/local/apache2/conf/server.key

# Update the default Apache SSL configuration with your certificate paths
RUN sed -i \
        -e 's|SSLCertificateFile .*|SSLCertificateFile "/usr/local/apache2/conf/server.crt"|g' \
        -e 's|SSLCertificateKeyFile .*|SSLCertificateKeyFile "/usr/local/apache2/conf/server.key"|g' \
        /usr/local/apache2/conf/extra/httpd-ssl.conf

# Include the SSL configuration in the main Apache configuration file
RUN echo 'Include conf/extra/httpd-ssl.conf' \
    >> /usr/local/apache2/conf/httpd.conf
RUN echo 'LoadModule ssl_module modules/mod_ssl.so' \
    >> /usr/local/apache2/conf/httpd.conf
RUN echo 'LoadModule socache_shmcb_module modules/mod_socache_shmcb.so' \
    >> /usr/local/apache2/conf/httpd.conf

# Expose port 80 for HTTP and port 443 for HTTPS
EXPOSE 80
EXPOSE 443

# Start Apache in the foreground
CMD ["httpd-foreground"]
