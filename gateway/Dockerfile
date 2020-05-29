FROM golang:latest

ARG GOOOS
ARG GOARCH
ENV NGROK_DOMAIN "ngrok.local"

WORKDIR /code

COPY keys/ /code

RUN apt-get update

RUN apt-get install build-essential git mercurial curl nodejs -y

RUN git clone https://github.com/inconshreveable/ngrok

RUN cp rootCA.pem /code/ngrok/assets/client/tls/ngrokroot.crt

RUN cd ngrok && make release-server
RUN cd ngrok && GOARCH=$GOARCH GOOS=$GOOS make release-client

EXPOSE 8001/tcp
EXPOSE 8000/tcp

CMD /code/ngrok/bin/ngrokd -tlsKey=/code/device.key -tlsCrt=/code/device.crt -domain="$NGROK_DOMAIN" -httpAddr=":8000" -httpsAddr=":8001"
