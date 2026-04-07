# TCP to HTTP

A hands-on Go project that builds an HTTP server from scratch using TCP sockets.

## Overview

This project is a progressive tutorial that walks through building an HTTP server at the protocol level. Rather than using libraries like `net/http`, each layer of HTTP is implemented from scratch, giving a deep understanding of how HTTP actually works.

## Structure

The directories are numbered to follow the learning progression:

| Directory | Topic |
|-----------|-------|
| `1-HTTP-streams` | Reading streaming data line-by-line |
| `2-TCP` | Basic TCP socket programming |
| `3-Requests` | Accepting and handling TCP connections |
| `4-Request-Lines` | Parsing HTTP request lines (GET /path HTTP/1.1) |
| `5-HTTP-Headers` | Parsing HTTP headers |
| `6-HTTP-Body` | Reading request bodies |
| `7-HTTP-Response` | Building and sending HTTP responses |

## Running

Each directory is a complete Go module. To run the HTTP server from any chapter:

```bash
cd 7-HTTP-Response
go run cmd/httpserver/main.go
```

Or run the TCP listener (reads raw bytes):

```bash
go run cmd/tcplistener/main.go
```

## Testing

Run tests for any chapter:

```bash
cd 5-HTTP-Headers
go test ./...
```

## What You'll Build

By the end, you'll have a working HTTP server that:
- Accepts TCP connections
- Parses the HTTP request line and headers
- Reads request bodies
- Returns proper HTTP responses with status codes
