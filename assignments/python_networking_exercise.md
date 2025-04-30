# Python Networking Exercises

A collection of five beginner-friendly Python networking exercises for WSL. Each should take no longer than one hour to complete.

## Prerequisites

- WSL (Ubuntu or other distro) installed and running  
- Python 3.x and pip:  
  ```bash
  sudo apt update
  sudo apt install -y python3 python3-pip
  ```
- `requests` library:  
  ```bash
  pip3 install requests
  ```

## Exercises

1. **DNS Lookup** (`dns_lookup.py`)
   - Prompt the user for a domain name.
   - Use `socket.gethostbyname()` to fetch the IPv4 address.
   - Use `socket.gethostbyaddr()` to retrieve the canonical hostname.

2. **HTTP GET & Headers** (`http_get_headers.py`)
   - Perform an HTTP GET request to a specified URL (e.g., https://httpbin.org/get).
   - Display status code, `Content-Type` header, and the first 200 bytes of the response.

3. **TCP Echo Server & Client** (`tcp_echo_server.py`, `tcp_echo_client.py`)
   - Server listens on `localhost:5000`. Accepts one connection and echoes received data.
   - Client connects, sends a message, and prints the response.

4. **UDP “Ping” Server & Client** (`udp_server.py`, `udp_client.py`)
   - Server binds to `localhost:5001` and replies “pong” to any incoming message.
   - Client sends “ping” and prints the server’s response.

5. **Mini Port Scanner** (`port_scanner.py`)
   - Scans a range of ports on a given host using `socket.connect_ex()`.
   - Prints open ports.

## Usage

1. **Open two (or more) WSL terminals** for server/client where needed.  
2. Run scripts with `python3`:
   ```bash
   python3 dns_lookup.py
   python3 tcp_echo_server.py
   python3 tcp_echo_client.py
   ```
3. For the port scanner:
   ```bash
   python3 port_scanner.py
   ```
4. No special permissions are needed (all ports ≥5000).  
