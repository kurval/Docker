# Clone rails app and build:
# $ docker build -t ft-rails:on-build .

# Create the Dockerfile from the subject:
# $ echo -e 'FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]' > Dockerfile_Subject

# Then build subject file:
# $ docker build -t ex02 -f Dockerfile_Subject .

# And run:
# $ docker run -it -p 3000:3000 --rm ex02

FROM ruby:2.5.1

RUN apt-get update && \
	apt-get install -y nodejs \
	build-essential \
	libpq-dev \
	sqlite3 \
	yarn

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD RUN bundle update
ONBUILD RUN bundle install
ONBUILD RUN rake db:drop
ONBUILD RUN rake db:create 
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed
