# Vouqis

Vouqis is a CLI tool that tests the reliability of Model Context Protocol (MCP) servers. It runs 10 deterministic JSON-RPC probes and generates a 0 to 100 trust score in 30 seconds.

Standard HTTP monitoring misses protocol-layer failures. An MCP server might return a 200 OK status while silently accepting malformed requests or returning empty bodies. Vouqis detects these silent failures before they break downstream agent workflows.

## Installation

Vouqis requires Node.js 18 or higher. Install the CLI globally:

```bash
npm install -g @vouqis/cli
```

## Usage

Run a single audit against any reachable MCP server URL:

```bash
vouqis audit https://your-mcp-server
```

Every audit automatically generates a shareable report URL. 

To block CI/CD deployments when reliability drops, set a minimum trust score threshold:

```bash
vouqis audit $MCP_URL --fail-below 80
```

## How it Works

The tool sends specific JSON-RPC requests and evaluates the server response without using LLMs. It tests five failure modes:

* **Malformed Requests:** Checks if the server correctly rejects invalid JSON structure.
* **Missing Parameters:** Confirms the server returns an error when required inputs are absent.
* **Latency:** Verifies responses arrive within 500 milliseconds.
* **Schema Compliance:** Ensures outputs follow the JSON-RPC 2.0 specification.
* **Empty Responses:** Detects null or empty response bodies.

## Trust Scores

Vouqis weights the pass rate (50%), latency (30%), and error diversity (20%) to assign a score. 

* **80 to 100 (Approved):** Safe to integrate. Monitor during deployments to catch regressions.
* **50 to 79 (Risky):** Acceptable for non-critical tasks. Review the failed probes before production use.
* **0 to 49 (Do Not Integrate):** High probability of causing agent failures. Critical reliability issues detected.

## Project Information

Vouqis offers a free tier for single audits and 30-day public report history. A Pro tier is available for API keys, CI/CD integration, and 90-day history. 

Built by Sasi. For support, contact sasisundhar2211@gmail.com
