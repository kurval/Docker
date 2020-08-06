# clone simple rails application named "app"
# $ docker build -t ft-rails:on-build .

## Create the Dockerfile:
# $ echo -e 'FROM ft-rails:on-build\nEXPOSE 3000\nCMD ["rails", "s", "-b", "0.0.0.0", "-p" ,"3000"]' > Dockerfile_Subject

## And run:
# $ docker build -t ex02 -f Dockerfile_Subject .
# $ docker run -it -p 3000:3000 --rm ex02

FROM ruby:2.5.1

RUN apt-get update && apt-get install -y nodejs=4.8.2~dfsg-1 && rm -rf /var/lib/apt/lists/*
RUN gem install rails bundler

COPY app /opt/app
WORKDIR /opt/app

ONBUILD RUN bundler install
ONBUILD RUN rails db:migrate
ONBUILD RUN rails db:seed

# check <ip of Char>:3000
