## clone rails app
# $ docker build -t ft-rails:on-build .
## Create the Dockerfile from the subject
# $ echo -e 'FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]' > Dockerfile_Subject
## Then run subject build:
# $ docker build -t ex02 -f Dockerfile_Subject .
# $ docker run -it -p 3000:3000 --rm ex02

FROM ruby:2.5.1

RUN apt-get update -y && \
	apt-get install -y 	nodejs && \
	apt-get install -y  build-essential && \
	apt-get install -y  libpq-dev && \
	apt-get install -y  sqlite3 && \
	apt-get install -y  yarn

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD RUN bundle update
ONBUILD RUN bundle install
ONBUILD RUN rake db:drop
ONBUILD RUN rake db:create 
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed
