# Hardware arquitecture
In hardware the closest thing as a brain is a CPU ( Central Processing Unit ), it is designed to answer 3.2 billions 
per second, where the instructions enter into the pins of the CPU, and we store the instructions in the main memory(RAM), so
the CPU asks what's next and the main memory stores the answers and the questions. The difference between the secondary 
memory and the main memory is that the secondary storage is not vanished, it can store data longer.
Our program is loaded and stored in the main memory.
# Networked Programs
- TCP (Transport Control Protocol):
  - Built on top of IP
  - Assumes Ip might lose some data
    - store s and retransmits data if it seems to be lost
    - handles "flow control" using a transmit window
    - Provides a nice reliable pipe (pipe is uni-directional)
- **Sockets**: Is an endpoint of a bidirectional inter-process communication flow across
  an Internet Protocol-base computer network, such as the internet ( between 2 machines of the same machine itself )
  - They allow communication between 2 machines or the same itself
  - They are the combination of port and IP address
    - The IP identifies the device (computer or server)
    - The port identifies a specific application or server running in that device
  - A socket is an end-point of communication between 2 devices
  - Process <-> Internet <-> Process
    - which is similar to SocketA <-> Internet <-> SocketB
    - One process in one computer, another process running in a second computer connected by the internet
  - Sockets use the client-server model, where one device acts as a server, waiting for incoming connections, and the other device acts as a client, initiating the connection to the server. Once the connection is established, both the server and client can send and receive data through their respective sockets.
- **TCP Port Numbers**: 
  - a port is an application-specific or process-specific software communications endpoint..
  - It allows multiple networked applications to coexist on the same server 
  - There is a list of well-known TCP port numbers
  - Common TCP Ports;
    - Telnet(23): Login
    - SSH(22): Secure login
    - HTTP(80): We are expecting to talk to a web server
    - HTTPS(443): Secure
    - SMTP(25): (Mail)
    - IMAP(143/220/993): Mail
    - POP(109/110): Mail Retrieval
    - DNS(53): Domain name
    - FTP(21): File transfer
```py
import socket
mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM) # Creates the socket
mysock.connect(('data.pr4e.org',80)) # Connects the socket, associate it 
# it connects to the host (data.pr4e.org) and the port(80) where some process is ocurring
# It haven't send any data yet, it's just the connection to the server port
```
The application is creating a socket and then connect it to and appliction|process running on 
the port of that host(which is the socket of another server|computer).

# Application protocols

- MAIL: is another appliction protocol
- HTTP: Hypertext Transfer Protocol, is the set of rules to allow browsers to retrieve web documents from servers over the internet

They are the way of what we send and what we recieve, looking at a typical url
```
http://www.mefercs.com/page1.html

protocol: http://
host: www.mefercs.com
document: page1.html
```
Here we are not running a process, that's way we don't have a port, so we are 
indicating the way in which we want to talk(the protocol) with the server(the host).

- As an example, an application(the browser) running on a port of my computer(the host/ip), has a socket.

We can create an http request using the next command line
```
$ telnet www.dr-chuck.com 80
GET http://www.dr-chuck.com/page1.htm HTTP/1.0
```
Which specifies a protocol(the way to talk to the server which is a login protocol) to the host(www.dr-chuck.com) at the port(the application running at) 80.
So the recap, we are saying that we want to talk with the telnet application protocol with the application running on the port 80 of the host www.dr-chuck.com,
and then we want to say that we want to get talking with the http protocol the page1 of the host.

- HTTP request in python, which is an small web browser
```py
import socket

mysock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
mysock.connect(('data.pr4e.org', 80))
cmd = 'GET http://data.pr4e.org/romeo.txt HTTP/1.0\r\n\r\n'.encode() # Cast the string(unicode) to bytes(UTF-8 bytes)
mysock.send(cmd) # We send bytes to the external socket (host and port)

while True:
    data = mysock.recv(512) # Recieving UTF-8 bytes
    if len(data) < 1:
        break
    print(data.decode(),end='') # Cast the UTF-8 Bytes to unicode 
mysock.close() # Finally we cut the connection to the external server but we still having the socket
```

# URLLIB

Url handlers the sockets for us and makes web pages look like a file.
```py
import urllib.request, urllib.parse, urllib.error
fhand = urllib.request.urlopen('http://data.pr4e.org/romeo.txt')
for line in fhand:
    print(line.decode().strip())
```
We could also receive html, but to parse that html we need to use another thing.

# SQLite

Using DBBRosew for Sqlite, we create a new DB
- [Dr-Chuck handout](https://www.py4e.com/lectures3/Pythonlearn-15-Database-Handout.txt)
- INSERT:
  ```sql
   INSERT INTO Users(name,email) VALUES (Miguel, example@gmail.com) 
  ```
### Multiple tables

- Integer reference Pattern: WE use integers to refrecene rows in another table
  - Primary key: Generally an integer auto-increment field
  - logical key: What the outised world uses for lookup
  - Foreing key: Generally an integer key pointing to a row in another table
  ```
  Album
  id // Primay key
  title //logical key
  artist_id //foreing key
  ```


