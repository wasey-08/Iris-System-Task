# Iris-System-Task

Task-1

steps;

1) Create a Dockerfile

2) Edit the Dockerfile and add the lines given below;

[
FROM ruby:3.0.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
WORKDIR /app
COPY Gemfile* ./
RUN bundle install
COPY . .
EXPOSE 3000
CMD ["rails","server","-b","0.0.0.0"]
]
where "0.0.0.0" will be your localhost

also refer to; ! [ screenshot of Dockerfile ](./images/Dockerfile.png "Dockerfile")
