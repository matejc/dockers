FROM golang:1.11

RUN wget -O /bin/dep https://github.com/golang/dep/releases/download/v0.5.0/dep-linux-amd64 && chmod +x /bin/dep

RUN mkdir -p $GOPATH/src/github.com/bitly/oauth2_proxy

RUN git clone git://github.com/bitly/oauth2_proxy $GOPATH/src/github.com/bitly/oauth2_proxy

WORKDIR $GOPATH/src/github.com/bitly/oauth2_proxy

ARG REPO_REV="a94b0a8b25e553f7333f7b84aeb89d9d18ec259b"

RUN git checkout $REPO_REV

RUN sed -i 's|gopkg.in/fsnotify.v1|gopkg.in/fsnotify/fsnotify.v1|' ./Gopkg.lock && \
  sed -i 's|gopkg.in/fsnotify.v1|gopkg.in/fsnotify/fsnotify.v1|' ./Gopkg.toml && \
  sed -i 's|github.com/18F/hmacauth|github.com/mbland/hmacauth|' ./Gopkg.toml && \
  sed -i 's|b502c41a61115d14d6379be26b0300f65d173bdad852f0170d387ebf2d7ec173|cfdd05348394cd0597edb858bdba5681665358a963356ed248d98f39373c33b5|' ./Gopkg.lock && \
  sed -i 's|gopkg.in/fsnotify.v1|gopkg.in/fsnotify/fsnotify.v1|' ./watcher.go

RUN dep ensure
RUN CGO_ENABLED=0 go build -ldflags="-s -w" -o bin/oauth2_proxy

FROM alpine:3.8

COPY --from=0 /go/src/github.com/bitly/oauth2_proxy/bin/oauth2_proxy /bin/oauth2_proxy

ENTRYPOINT [ "/bin/oauth2_proxy" ]
