FROM yourgrigor/ruby:2.1.3

RUN apt-get install -qq -y nodejs

RUN apt-get install -qq -y software-properties-common

RUN add-apt-repository -y ppa:nginx/stable
RUN apt-get update
RUN apt-get install -qq -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/nginx

ADD nginx-sites.conf /etc/nginx/sites-enabled/default

RUN gem install foreman

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --force-yes libpq-dev

RUN apt-get install -qq -y libmysqlclient-dev

WORKDIR /app
ONBUILD ADD Gemfile /app/Gemfile
ONBUILD ADD Gemfile.lock /app/Gemfile.lock
ONBUILD RUN bundle install --without development test
ONBUILD ADD . /app

ADD unicorn.rb /app/config/unicorn.rb

ADD Procfile /app/Procfile

ENV RAILS_ENV production

CMD bundle exec rake assets:precompile && foreman start -f Procfile
