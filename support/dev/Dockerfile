# matches https://pages.github.com/versions
# pinned to sha for reproducability
FROM ruby:2.7.4-buster@sha256:e4e6b92176de599ca2a5e36f932e5cc3e43a3e58cb82e2fd229fa3e32ff881ca

 # ruby base doesn't specify locale https://github.com/docker-library/ruby/issues/45
ENV LANG C.UTF-8

# Note debian and ubuntu based containers automatically run `apt-get clean` post install
# hadolint ignore=DL3008
RUN apt-get update \
 && apt-get install -y --no-install-recommends hunspell \
 && rm -rf /var/lib/apt/lists/*
