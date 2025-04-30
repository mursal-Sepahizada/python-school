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

## Solutions

### 1. DNS Lookup (`dns_lookup.py`)
```python
import socket

domain = input("Enter domain: ")
try:
    ip = socket.gethostbyname(domain)
    print(f"IPv4 address of {domain}: {ip}")
    hostname, _, _ = socket.gethostbyaddr(ip)
    print(f"Canonical hostname: {hostname}")
except socket.error as e:
    print(f"Error: {e}")
```

### 2. HTTP GET & Headers (`http_get_headers.py`)
```python
import requests

url = input("Enter URL (default https://httpbin.org/get): ") or "https://httpbin.org/get"
resp = requests.get(url)
print(f"Status code: {resp.status_code}")
print(f"Content-Type: {resp.headers.get('Content-Type')}")
print("Body (first 200 bytes):")
print(resp.content[:200])
```

### 3. TCP Echo Server & Client

#### `tcp_echo_server.py`
```python
import socket

HOST = '127.0.0.1'
PORT = 5000

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, PORT))
    s.listen(1)
    print(f"Listening on {HOST}:{PORT}...")
    conn, addr = s.accept()
    with conn:
        print(f"Connected by {addr}")
        while True:
            data = conn.recv(1024)
            if not data:
                break
            conn.sendall(data)
```

#### `tcp_echo_client.py`
```python
import socket

HOST = '127.0.0.1'
PORT = 5000
message = input("Message to send: ")

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    s.sendall(message.encode())
    data = s.recv(1024)
    print(f"Received: {data.decode()}")
```

### 4. UDP “Ping” Server & Client

#### `udp_server.py`
```python
import socket

HOST = '127.0.0.1'
PORT = 5001

with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
    s.bind((HOST, PORT))
    print(f"UDP server listening on {HOST}:{PORT}...")
    while True:
        data, addr = s.recvfrom(1024)
        print(f"Received {data.decode()} from {addr}")
        s.sendto(b"pong", addr)
```

#### `udp_client.py`
```python
import socket

HOST = '127.0.0.1'
PORT = 5001

with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
    s.sendto(b"ping", (HOST, PORT))
    data, _ = s.recvfrom(1024)
    print(f"Received: {data.decode()}")
```

### 5. Mini Port Scanner (`port_scanner.py`)
```python
import socket

host = input("Enter host (domain or IP): ")
start = int(input("Start port: "))
end = int(input("End port: "))

print(f"Scanning ports {start}-{end} on {host}...")
for port in range(start, end + 1):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.settimeout(0.5)
        result = s.connect_ex((host, port))
        if result == 0:
            print(f"Port {port} is open")
```
