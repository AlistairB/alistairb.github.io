# matches https://pages.github.com/versions
# pinned to sha for reproducability
FROM ruby:2.7.1@sha256:b3aab6a052e2397456342d97a4d0591d4b2bf54b1ee6e599867fb95440f22a6c

 # ruby base doesn't specify locale https://github.com/docker-library/ruby/issues/45
ENV LANG C.UTF-8

# Note debian and ubuntu based containers automatically run `apt-get clean` post install
# hadolint ignore=DL3008
RUN apt-get update \
 && apt-get install -y --no-install-recommends hunspell \
 && rm -rf /var/lib/apt/lists/*
