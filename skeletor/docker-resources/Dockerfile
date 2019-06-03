FROM node:10 as front-end

WORKDIR /app/
ADD docker-resources/application.tgz /app
RUN mkdir -p web/build
RUN if [ -f package.json ]; then \
    npm install && \
    npm run encore -- prod; fi

FROM connectholland/baseimages:php73-apache

WORKDIR /app/

ADD docker-resources/application.tgz /app
COPY --from=front-end /app/web/build /app/web/build
COPY docker-resources/composer-auth.json /root/.composer/auth.json
RUN if [ -f composer.json ]; then \
    composer install -o --no-dev --no-scripts; fi

COPY docker-resources/apache.conf /etc/apache2/sites-enabled/000-default.conf

COPY docker-resources/project-entrypoint /usr/local/bin/project-entrypoint

ENTRYPOINT ["project-entrypoint"]
