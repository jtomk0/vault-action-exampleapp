FROM ruby:latest
LABEL maintainer="HashiCorp Vault Education <team-vault-education@hashicorp.com>"

ARG wrapping_token="UNSET_TOKEN_PLEASE_OVERRIDE"
ARG retrieved_secret="UNSET_SECRET_PLEASE_OVERRIDE"

RUN apt-get update && \
    apt-get install -y net-tools

# Install gems
ENV APP_HOME /app
ENV HOME /root
RUN mkdir $APP_HOME
WORKDIR $APP_HOME
COPY ./vault-github-action/Gemfile* $APP_HOME/
RUN bundle install

# Upload source
COPY ./vault-github-action/config.ru $APP_HOME
RUN mkdir $APP_HOME/lib
COPY ./vault-github-action/lib/* $APP_HOME/lib

# Start server
ENV PORT 8080
EXPOSE 8080

RUN /bin/bash -c "echo '$wrapping_token' > ./wrapping_token"
RUN /bin/bash -c "echo '$retrieved_secret' > ./retrieved_secret"
CMD ["rackup", "--port", "8080", "--env", "production" ]
