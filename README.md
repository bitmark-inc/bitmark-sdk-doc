# Bitmark SDK Documentation

This is a bitmark SDK site which is hosted by slate.

## Build

This is a docker-based project. You can build the image via

```
docker build -t bitmark-sdk-doc .
```

## Run

You can run the documentation site by your local changes

```
docker run --rm -v /path/to/bitmark-sdk-doc/source:/slate/source -p 4567:4567 bitmark-sdk-doc
```
