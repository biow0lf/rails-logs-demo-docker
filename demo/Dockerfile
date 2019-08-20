FROM ruby:2.6.3-slim

RUN apt-get update -y && \
    apt-get dist-upgrade -y && \
    apt-get install gnupg2 git gcc make --no-install-recommends -y

RUN apt-get autoremove -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /app

WORKDIR /app

COPY Gemfile Gemfile

COPY Gemfile.lock Gemfile.lock

ENV RAILS_ENV production

ENV BUNDLER_VERSION 2.0.2

RUN gem install bundler --version "$BUNDLER_VERSION" --force

# throw errors if Gemfile has been modified since Gemfile.lock
RUN bundle config --global frozen 1

# two jobs
RUN bundle config --global jobs 2

# install only production gems without development and test
RUN bundle config --global without development test

# retry 5 times before fail
RUN bundle config --global retry 5

RUN bundle install

COPY . .

EXPOSE 3000

CMD ['rails', 'server', '-b', '0.0.0.0']
