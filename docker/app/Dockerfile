FROM ruby:2.7.2

# Install nodejs
RUN apt-get update -qq && apt-get install -y nodejs

# Add Yarn repository
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

# Update & Install Yarn
RUN apt-get update && apt-get install -y --no-install-recommends nodejs yarn

# Set an environment variable where the Rails app is installed to inside of Docker image:
ENV RAILS_ROOT /var/www/rails_ecr_sample_app/
RUN mkdir -p $RAILS_ROOT

# Set working directory
WORKDIR $RAILS_ROOT

# Install gems
COPY Gemfile* $RAILS_ROOT
# Run bundler with typical CI/CD options: jobs=4, retry=3
RUN gem install bundler:'~> 2.1.4'
RUN bundle install -j4 --retry 3

COPY . .

RUN bundle exec rake assets:precompile

EXPOSE 3000

CMD ./docker/docker-entrypoint.sh
