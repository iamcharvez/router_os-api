# mikos_api.py

**Version:** 1.0.0

## Overview

`mikos_api.py` is a pure Python client for the MikroTik RouterOS API. It allows you to connect to a RouterOS device, authenticate, and send/receive commands using the RouterOS API protocol. The script can be used as a standalone command-line tool or as a module in your own Python scripts.

---

## Features
- Connect to RouterOS devices over TCP (optionally with TLS)
- Authenticate using plain or challenge-response methods
- Send commands and receive structured replies
- Low-level implementation of the RouterOS API protocol

---

## Requirements
- Python 3.x
- No external dependencies (uses only the Python standard library)

---

## Installation
Copy `mikos_api.py` into your project directory.

---

## Usage

### As a Script
Run from the command line:

```bash
python3 mikos_api.py <host> [username] [password] [secure]
```
- `host`: RouterOS device address (IP or hostname)
- `username`: (optional) Username (default: admin)
- `password`: (optional) Password (default: empty)
- `secure`: (optional) Use TLS (1/true/yes for True)

Example:
```bash
python3 mikos_api.py 192.168.88.1 admin mypassword yes
```

### As a Module
Import and use in your own scripts:

```python
from mikos_api import ApiRos, open_socket

s = open_socket('192.168.88.1', 8728)
api = ApiRos(s)
if api.login('admin', 'password'):
    for reply, attrs in api.talk(['/system/identity/print']):
        print(reply, attrs)
```

---

## API Reference

### Class: ApiRos
A client for the RouterOS API.

#### Methods
- `__init__(self, sk)`: Initializes the API client with a socket object.
- `login(self, username, pwd)`: Authenticates with the RouterOS device using the provided username and password. Handles both plain and challenge-response authentication.
- `talk(self, words)`: Sends a command (as a list of words) to the device and yields replies.
- `writeSentence(self, words)`: Sends a list of words as a sentence to the device.
- `readSentence(self)`: Reads a sentence (list of words) from the device.
- `writeWord(self, w)`: Sends a single word to the device.
- `readWord(self)`: Reads a single word from the device.
- `writeLen(self, l)`: Encodes and sends the length of a word according to the RouterOS API protocol.
- `readLen(self)`: Reads and decodes the length of a word.
- `writeStr(self, str)`: Sends a string to the device.
- `writeByte(self, str)`: Sends raw bytes to the device.
- `readStr(self, length)`: Reads a string of the specified length from the device.

### Function: open_socket(dst, port=8728, secure=False)
Resolves the address and opens a TCP (optionally SSL/TLS) socket to the RouterOS device.
- `dst`: Hostname or IP address of the RouterOS device.
- `port`: Port number (default 8728 for plain, 8729 for TLS).
- `secure`: If True, uses SSL/TLS.

---

## Notes
- The script uses low-level socket operations and implements the RouterOS API protocol manually.
- For secure connections, Python's `ssl` module is used with TLSv1.2.
- The script prints replies from the device to stdout when run as a script.

---

## License
MIT License
