# Build stage
FROM arm64v8/ruby:3.2 as Builder
RUN apt-get update -qq && apt-get install -y nodejs default-mysql-client
ENV TZ=Asia/Tokyo
# RUN mkdir /mouretsu
WORKDIR /mouretsu
COPY Gemfile /mouretsu/Gemfile
COPY Gemfile.lock /mouretsu/Gemfile.lock
RUN gem update && \
    bundle install
COPY . /mouretsu

# Runtime stage
FROM arm64v8/ruby:3.2-slim
RUN apt-get update -qq && apt-get install -y nodejs default-mysql-client
ENV TZ=Asia/Tokyo
RUN mkdir /mouretsu
WORKDIR /mouretsu
COPY --from=Builder /usr/local/bundle/ /usr/local/bundle/
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0"]