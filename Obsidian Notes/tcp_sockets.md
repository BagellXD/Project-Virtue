
**import socket** -> importing library that allows you to use sockets(tcp communication protocol)

**import threading** -> allows user to use the modules that allow **multiple scripts on the same file to run in parallel to another. ( useful in dicts)**

```python
import socket
import threading
```
port  and the clients / nodes ip address allows clients to know which port they should go to and aswell as which address on the network it should visit

**EXAMPLE**:
**PORT** = 5050
**IP** = socket.gethostbyname(socket.gethostname())
-> 
	- Gets the computer's hostname.
	- Resolves that hostname into the computer's local IP address.
	- Useful when you don't want to hard-code your LAN IP (e.g. 192.168.x.x).
**IP** = "" 
-> 
	- Used when binding a server socket.
	- Tells the OS to bind to all available network interfaces.
	- This means the server will accept connections on any IP address assigned to the computer.
	- Equivalent to "0.0.0.0".

# Socket

**Sockets** allow a program to communicate over a network. Every device that wants to send or receive data first creates a socket.

->
```python
host = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

`AF_INET`
-> Specifies the address family the socket should use.
(In this case **IPv4**.)

`SOCK_STREAM`
-> Creates a **TCP** socket.
(TCP is reliable and connection-oriented.)

---

# .bind()

Associates a socket in this case **host** with an **IP address** and **port** so other devices know where to connect.

->
```python
host.bind((IP, PORT))
```

### Rules
1. The address **must** be a tuple:
   ```python
   (IP, PORT)
   ```
2. The **IP** always comes first.
3. The **PORT** always comes second.
4. Servers usually call `.bind()`, clients usually don't.
---
---
---
# .listen(backlog)

Starts listening for incoming TCP connection requests.

->
```python
server.listen(5)
```

### Notes
- Called once after `.bind()`.
- Does **not** accept clients.
- Allows incoming connections to wait in a queue until the server is ready.

---

# .accept()

Accepts the next client waiting in the queue.

->
```python
conn, addr = server.accept()
```

### Returns

`conn`
-> A **new socket** connected to that client. IN SHORTS ALLOW YOU TO COMMUNICATE TO THAT CLIENT SPECIFICALLY

`addr`
-> The client's IP address and port.

### Notes
- Blocks until a client connects.
- The listening socket continues waiting for more clients.
- All communication happens through `conn`, not `server`.
---

---
# Threading

**Threading** allows a program to run multiple tasks at the same time.

This is useful for servers because one client can be handled while the server continues accepting new connections.

## Creating a Thread

```python
thread = threading.Thread(target=client_handling, args=(conn, addr))
```

`target`
-> The function the thread should run.

`args`
-> The arguments passed to the target function.
**Must** be a tuple.

### Notes
- Creating a thread does **not** start it.
- It only prepares the thread.

## Starting a Thread

```python
thread.start()
```

Starts the thread and executes the target function.

### Notes
- The main program continues running.
- The target function runs in parallel with the main thread.
- You can not make a thread and start the thread at the same time aka the same code
```python
thread= threading.Thread(target = client_handling,args=(conn,addr)).start()
```

#### Number of threads
to count the number of threads made and active it is 
```python
threading,activeCount()
```
---
---
---

# Handling Clients

```python
def client_handling(conn, addr):
	pass
```

`conn`
-> The socket connected to a specific client. Used to `send()` and `recv()` data.

`addr`
-> The client's IP address and port.

### Communication between server and the client 

```python
msg_length = conn.recv(HEADER).decode(utf-8)
msg_length = int(msg_length.strip())
msg = conn.recv(msg_length).decode(utf-8)
```
->
```
recv() -> allows the serfver to recieve the text sent by the client
HEADER -> is the number of bytes expected to be recieved
.decode() -> decodes the message sent from byte to normal message factor
.decode(utf-8) -> utf-8 is the normal text factor
```
