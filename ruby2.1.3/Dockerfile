FROM yourgrigor/ruby:2.1.3

RUN apt-get update && gem install rails

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN bundle install && rails server
