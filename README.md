# TCP to HTTP

A hands-on Go project that teaches HTTP by building it directly on top of TCP sockets.

## Overview

This repository is organized as a step-by-step learning journey.  
Each folder is one chapter, and each chapter adds one new piece of HTTP behavior.

The goal is simple: understand what a web server does internally, not just how to use `net/http`.

---

## How this project is organized

The folders are numbered in learning order:

1. `1-HTTP-streams`
2. `2-TCP`
3. `3-Requests`
4. `4-Request-Lines`
5. `5-HTTP-Headers`
6. `6-HTTP-Body`
7. `7-HTTP-Response`
8. `8-Chunked-Encoding`
9. `9-Binary-Data`

Each chapter is its own Go module (`go.mod` inside each folder), so you can run and test chapters independently.

---

## Detailed chapter-by-chapter explanation

### 1) `1-HTTP-streams` — Read streamed text data safely

This chapter starts with file input, not networking.  
It teaches a core concept: incoming data arrives in chunks, and chunk boundaries are not line boundaries.

What it demonstrates:
- Reading data in fixed-size byte slices.
- Reconstructing complete lines across multiple reads.
- Emitting lines through a channel.

Why it matters:
- TCP works as a stream, exactly like this.  
- If you can correctly split lines from a stream, you can later parse HTTP request text.

---

### 2) `2-TCP` — Open a TCP listener and accept client connections

This chapter replaces file input with real socket input.

What it demonstrates:
- `net.Listen("tcp", ":42069")` to open a listening socket.
- `Accept()` loop to receive incoming clients.
- Reusing the line-reading stream logic from chapter 1.

Why it matters:
- This is the first time your program behaves like a server.
- You now handle live client bytes over a network connection.

---

### 3) `3-Requests` — Receive full incoming request streams

This chapter continues working with raw incoming connection data and prepares for structured parsing.

What it demonstrates:
- Keeping the accept/read flow stable.
- Treating each connection as an independent stream to process.
- Laying groundwork for parsing HTTP semantics.

Why it matters:
- Before parsing protocol fields, you need reliable connection and stream handling.

---

### 4) `4-Request-Lines` — Parse the HTTP request line

This chapter introduces structured HTTP parsing.

What it demonstrates:
- Parsing the first line of an HTTP request (for example: `GET /hello HTTP/1.1`).
- Extracting:
  - HTTP method
  - request target/path
  - HTTP version

Why it matters:
- The request line tells your server what the client wants.
- Without this, routing and response decisions are impossible.

---

### 5) `5-HTTP-Headers` — Parse and manage request headers

This chapter adds full header parsing support.

What it demonstrates:
- Reading header lines after the request line.
- Parsing `Header-Name: value` pairs.
- Storing and iterating headers through a dedicated headers package.

Why it matters:
- Many HTTP features depend on headers (content types, host, auth, transfer settings, etc.).
- Correct header parsing is required before body handling and response building.

---

### 6) `6-HTTP-Body` — Read request body content

This chapter adds body support for requests that carry payload data.

What it demonstrates:
- Reading the body after headers are parsed.
- Using header information (such as content length) to know how much to read.
- Printing request body content in the TCP listener example.

Why it matters:
- Methods like POST/PUT rely on request bodies.
- Body parsing is necessary for APIs, forms, and uploads.

---

### 7) `7-HTTP-Response` — Build and send valid HTTP responses

This chapter introduces server responses and basic routing behavior.

What it demonstrates:
- Writing status lines (200, 400, 500).
- Writing response headers (`Content-Length`, `Content-Type`).
- Writing HTML response bodies.
- Serving different responses based on request path.

Why it matters:
- This is the first chapter where your server is fully conversational: parse request, decide behavior, send proper HTTP response.

---

### 8) `8-Chunked-Encoding` — Stream responses with chunked transfer + trailers

This chapter introduces HTTP streaming responses.

What it demonstrates:
- Proxying data from `httpbin`.
- Sending response body in chunks (`Transfer-Encoding` style behavior).
- Ending the stream with a zero-size chunk.
- Sending trailer headers (e.g., SHA256 of streamed content, final length).

Why it matters:
- Not all responses have known length at start time.
- Chunked encoding is a core HTTP/1.1 mechanism for streaming.

---

### 9) `9-Binary-Data` — Serve real binary files and combine techniques

This chapter adds binary payload handling and combines previous concepts.

What it demonstrates:
- Serving an MP4 file (`9-Binary-Data/assets/vim.mp4`) with `Content-Type: video/mp4`.
- Correct binary-safe writes over the response stream.
- Keeping route-based behavior, including chunked proxy path and normal HTML paths.

Why it matters:
- Real servers return more than text: images, videos, downloads, archives, etc.
- This chapter proves your server can handle practical web content formats.

---

## How to run a chapter

Example (chapter 7):

```bash
cd 7-HTTP-Response
go run cmd/httpserver/main.go
```

For chapters that include only a TCP listener:

```bash
go run cmd/tcplistener/main.go
```

Then, in another terminal, you can send a request:

```bash
curl -i http://localhost:42069/
```

---

## How to test a chapter

Run tests from the chapter directory:

```bash
cd 5-HTTP-Headers
go test ./...
```

---

## What you will understand after finishing

By completing all chapters, you should clearly understand:
- how TCP streams are read and parsed,
- how HTTP request lines, headers, and bodies are structured,
- how to generate valid HTTP responses manually,
- how chunked transfer and trailers work,
- and how to serve binary content correctly.
