# child_repo_for_submodule_hello_world

A standalone, minimal Node.js standard-library HTTP "Hello, World!" server — byte-identical to the superproject's `server.js`. Source: server.js:L1-L14.

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Running the Server](#running-the-server)
4. [API Documentation](#api-documentation)
5. [Code Explanation](#code-explanation)
6. [Deployment Guide](#deployment-guide)
7. [Relationship to Superproject](#relationship-to-superproject)
8. [Diagram](#diagram)
9. [Source Citations](#source-citations)

## Overview

This repository is a standalone, dependency-free HTTP server implemented entirely with the Node.js built-in `http` module. Source: server.js:L1.

It binds to the IPv4 loopback interface and responds to **every** request with a fixed `200 OK` plain-text greeting. Source: server.js:L6-L10.

The `server.js` in this repository is **byte-identical** to the superproject's `server.js`; the repository is tracked as a Git submodule and serves as a submodule-materialization validation fixture. Source: server.js:L1-L14.

## Prerequisites

- **Node.js** — any maintained LTS release that supports CommonJS (`require`) and the built-in `http` module. No specific version is pinned: this repository has **no** `package.json` and **no** `.nvmrc`, so do not assume a specific major version is required. Source: server.js:L1.
- **Git** — required to materialize this repository as a submodule of the superproject. Source: .gitmodules.
- **No `npm install`** — there are **zero** third-party dependencies; the only dependency is the Node.js built-in `http` module, so there is nothing to install. Source: server.js:L1.

## Running the Server

From the repository root, start the server directly with the Node.js runtime — `server.js` is a single, self-contained script that needs no build step or dependency installation. Source: server.js:L1-L14.

```bash
node server.js
```

Once it binds successfully, it prints the following confirmation line to stdout:

```text
Server running at http://127.0.0.1:3000/
```

Source: server.js:L13.

## API Documentation

The server exposes a single **catch-all** HTTP responder — there is no routing, no separate endpoints, and no `404` handling. Source: server.js:L6-L10.

| Aspect | Value | Source |
|---|---|---|
| Methods | ALL (any method) | server.js:L6-L10 |
| Paths | ALL (any path) | server.js:L6-L10 |
| Status | `200 OK` | server.js:L7 |
| Content-Type | `text/plain` | server.js:L8 |
| Body | `Hello, World!\n` | server.js:L9 |
| Content-Length | `14` | server.js:L9 |

**Catch-all behavior:** every request — regardless of HTTP method or URL path — converges on the exact same `200 OK` `text/plain` response with the body `Hello, World!\n`. The handler never inspects the request method, URL, headers, or body. Source: server.js:L6-L10.

**Example — `GET /`:**

```bash
curl -i http://127.0.0.1:3000/
```

**Example — non-GET request to an arbitrary path (`POST /anything/else`):**

```bash
curl -i -X POST http://127.0.0.1:3000/anything/else
```

Both commands return the **identical** response shown below, demonstrating the catch-all behavior:

```text
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Length: 14

Hello, World!
```

Source: server.js:L6-L10.

## Code Explanation

A line-by-line walkthrough of all 14 lines of `server.js`. All line references below are to this repository's sibling `server.js`.

| Line | Statement | Explanation |
|---|---|---|
| `server.js:L1` | `const http = require('http');` | Imports Node's built-in `http` module — the only dependency (no third-party packages). |
| `server.js:L2` | *(blank line)* | Intentional separator. |
| `server.js:L3` | `const hostname = '127.0.0.1';` | Defines the bind address as the IPv4 loopback interface (local-only). |
| `server.js:L4` | `const port = 3000;` | Defines the TCP port the server listens on. |
| `server.js:L5` | *(blank line)* | Intentional separator. |
| `server.js:L6` | `const server = http.createServer((req, res) => {` | Creates the HTTP server and opens the catch-all request handler. |
| `server.js:L7` | `res.statusCode = 200;` | Sets the response status to `200 OK` for every request. |
| `server.js:L8` | `res.setHeader('Content-Type', 'text/plain');` | Sets the response `Content-Type` header to `text/plain`. |
| `server.js:L9` | `res.end('Hello, World!\n');` | Writes the response body `Hello, World!\n` and ends the response. |
| `server.js:L10` | `});` | Closes the request-handler callback. |
| `server.js:L11` | *(blank line)* | Intentional separator. |
| `server.js:L12` | `server.listen(port, hostname, () => {` | Binds the server to `hostname:port` and registers the startup callback. |
| `server.js:L13` | ``console.log(`Server running at http://${hostname}:${port}/`);`` | Logs the startup confirmation line to stdout once the server is bound. |
| `server.js:L14` | `});` | Closes the `server.listen` callback. |

## Deployment Guide

- **Loopback-only.** The server binds to `127.0.0.1`, so it is reachable **only** from the local machine and is **not** exposed to other hosts on the network. Source: server.js:L3.
- **Port-3000 conflict.** This submodule server and the byte-identical superproject server both bind `127.0.0.1:3000`, so they **cannot run concurrently** — starting the second process fails with `EADDRINUSE`. Source: server.js:L3-L4.
- **No build step.** There is nothing to compile or bundle; run the file directly with `node server.js`. Source: server.js:L1-L14.
- **Process management (optional).** To keep the process running unattended, you may launch `node server.js` under any standard OS service manager or process supervisor. This adds no capability to the server itself — the runtime behavior remains the fixed catch-all response. Source: server.js:L6-L10.

## Relationship to Superproject

This repository is tracked as a **Git submodule** of the parent superproject. The superproject pins it at a specific commit recorded by the superproject's **gitlink** — not in `.gitmodules`, which stores only the submodule path and URL. To see the exact pinned commit, run `git submodule status` (or `git ls-tree HEAD child_repo_for_submodule_hello_world`) from the superproject root. The upstream URL is `https://github.com/lakshya-blitzy/child_repo_for_submodule_hello_world.git`, and this repository contains **no** nested submodules (it has no `.gitmodules` of its own). Source: superproject `.gitmodules` (path and URL); superproject gitlink via `git submodule status` (pinned commit).

For superproject-level setup — cloning with `--recurse-submodules`, the materialization workflow, and the overall architecture — see the [superproject README](../README.md). Source: .gitmodules.

## Diagram

The sequence diagram below shows that all methods and all paths converge on one fixed response, served from the loopback bind address `127.0.0.1:3000` shown in the diagram. Source: server.js:L3-L4 (bind address and port), server.js:L6-L10 (catch-all response).

```mermaid
sequenceDiagram
    participant Client
    participant Server as Node http server (127.0.0.1:3000)
    Client->>Server: HTTP request (any method, any path)
    Server-->>Client: 200 OK, Content-Type text/plain, "Hello, World!\n"
```

## Source Citations

- `child_repo_for_submodule_hello_world/server.js:L1-L14`
- `.gitmodules` (superproject) — submodule binding metadata: submodule **path** and upstream **URL** only; it does **not** record the pinned commit.
- Superproject **gitlink**, via `git submodule status` / `git ls-tree HEAD child_repo_for_submodule_hello_world` — the **pinned commit** the superproject records for this submodule.
