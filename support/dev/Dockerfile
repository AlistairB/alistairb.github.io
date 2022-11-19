# matches https://pages.github.com/versions
# pinned to sha for reproducability
FROM ruby:2.7.4@sha256:c4f29f28d297efcce62455a20d38383155a85eda9de38ed4635c2c5f3017df03

 # ruby base doesn't specify locale https://github.com/docker-library/ruby/issues/45
ENV LANG C.UTF-8

# Note debian and ubuntu based containers automatically run `apt-get clean` post install
# hadolint ignore=DL3008
RUN apt-get update \
 && apt-get install -y --no-install-recommends hunspell \
 && rm -rf /var/lib/apt/lists/*
