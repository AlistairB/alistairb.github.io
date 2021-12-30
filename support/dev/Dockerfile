# matches https://pages.github.com/versions
# pinned to sha for reproducability
FROM ruby:2.7.3@sha256:ee2b19ced1d44600963c52691b9b6975e209286e7168230d03afe7894f9c81a0

 # ruby base doesn't specify locale https://github.com/docker-library/ruby/issues/45
ENV LANG C.UTF-8

# Note debian and ubuntu based containers automatically run `apt-get clean` post install
# hadolint ignore=DL3008
RUN apt-get update \
 && apt-get install -y --no-install-recommends hunspell \
 && rm -rf /var/lib/apt/lists/*
