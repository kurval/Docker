FROM alpine:latest

RUN apk update && \
	apk upgrade && \
	apk add vim

ENTRYPOINT vim

# Build Image
# docker build -t ex00 .

# Create Container
# docker run --name ex00 -it --rm ex00
