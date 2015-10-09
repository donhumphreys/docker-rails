FROM donhedgeye/ruby:1.9.3-p545

# install rails dependencies
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    nodejs \
    mysql-client \
    postgresql-client \
    sqlite3 \
  && rm -rf /var/lib/apt/lists/*

# install rails
ENV RAILS_VERSION 4.0.13
RUN gem install rails --version "$RAILS_VERSION"

# throw errors if Gemfile has been modified since Gemfile.lock
RUN bundle config --global frozen 1

# create directory to store running app
RUN mkdir -p /app
WORKDIR /app

# install bundle from app Gemfile
ONBUILD COPY Gemfile /app/
ONBUILD COPY Gemfile.lock /app/
ONBUILD RUN bundle install

# copy application files to image
ONBUILD COPY . /app

# default to running rails server on port 3000
EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0"]
