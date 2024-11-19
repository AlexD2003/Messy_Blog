+++
author = 'Alex Domnit'
title = 'Socket'
date = 2024-11-19T14:23:43+02:00
draft = false
description = "Homework"
tags = [
    "Completed",
]
categories = [
    "UBB",
]
+++

### Requirements

Bidirectional client+server, UDP and TCP.\
More than one programming language.\
Working on multiple OS`s.\
Concurency.\
GUI implementation.

### Doc

[TCP vs UDP](https://www.spiceworks.com/tech/networking/articles/tcp-vs-udp/)\
TCP is connection-oriented while UDP is connectionless The connection is established via a three-way handshake. The client sends a synchronization request, the server sends back an acknowledgment, and the client returns a synchronization acknowledgment in response. Comparatively, UDP is a connectionless protocol.\
[What is a socket?](https://www.geeksforgeeks.org/socket-in-computer-network/)\
[TK tutorial](https://www.geeksforgeeks.org/python-tkinter-tutorial/)

### Solution

#### TCP implementation

Since a later requirement is utilizing multiple programming languages, I\`ll start off by making the client and server in 2 different programmign languages.\
Firstly, let\`s implement the **server.py** functionality since it is the simpler of the 2. The logic behind it is that the python server listens for incoming connections, receives data from the designated client and then spits out a response.

**server.py :**
```
import socket

HOST = '0.0.0.0'  
PORT = 65432

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, PORT))
    s.listen()
    print(f"Server listening on {HOST}:{PORT}...")
    conn, addr = s.accept()
    with conn:
        print(f"Connected by {addr}")
        data = conn.recv(1024)
        print(f"Received: {data.decode()}")
        conn.sendall(b"Server default message.")
```

A simple breakdown:\
**socket.AF_INET** tells the server that it will use **IPV4** as a means of comunication.\
**socket.SOCK_STREAM** specifies that the communication protocol will be **TCP**.\
**bind** is used in order to bind a specific **IP address** and **port** which in this case is 0.0.0.0 on port 65432.\
**listen** listens for connections.\
**accept** accepts the connection when one is trying to be established with the server and then the data is being decoded from bytes into a string.\
**sendall** is utilized for the response to the client.

**client.cpp:**
```
#include <iostream>
#include <cstring>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>

int main() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char buffer[1024] = {0};

    if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        std::cerr << "Socket creation error\n";
        return -1;
    }

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(65432); 

    if (inet_pton(AF_INET, "0.0.0.0", &serv_addr.sin_addr) <= 0) {
        std::cerr << "Invalid address\n";
        return -1;
    }

    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        std::cerr << "Connection failed\n";
        return -1;
    }

    std::string message = "Client message!";
    send(sock, message.c_str(), message.size(), 0);
    std::cout << "Message sent\n";

    int valread = read(sock, buffer, 1024); 
    std::cout << "Server reply: " << buffer << "\n";

    close(sock);
    return 0;
}
```

Client breakdown:\
**socket(AF_INET, SOCK_STREAM, 0)** creates a **TCP socket** with the required specifications.\
Then, whe setup the IP and port we want to achieve the connection through.\
**connect** tries to establish a connection between the client and the server.\
**send** is used in order to send the message.\
**read** receives data from the server.\
**close** in order to close the connection afterwards.

#### Running

By opening up 2 terminals and running the the TCP server and client, we can see that both connections are established locally and the messages get through each other.

**Terminal 1:**
```
03:15:34 archie@Archie Socket → python3 server.py
Server listening on 0.0.0.0:65432...
Connected by ('127.0.0.1', 45098)
Received: Client message!
```

**Terminal 2:**
```
03:15:08 archie@Archie Socket → g++ -o client client.cpp
03:15:11 archie@Archie Socket → ./client
Message sent
Server reply: Server message!
```

In order to test the compliance between different OS\`s, I\`ve booted up the **server.py** on my windows machine, and got a response by modifying the IPV4 adress with the adress of the windows machine on my local network.

<img src="/img/socket_1.jpg" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

<img src="/img/socket_2.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

#### TCP to UDP

The transition is pretty straight forward, firstly we need to change the socket type from **SOCK_STREAM** to **SOCK_DGRAM** and since UPD is conectionless, we no longer need **connect** and **accept**. Since UDP doesn`t guarantee connection, we need to establish a connection. We simply need to use **sendto** and **recfrom**.

#### Multithreading and TCP/UDP duality

Now, it`s time to update the code in order to work with both UDP or TCP based on our desired choice and for the IP adress to be an argument, not a defined value. After that, in order to implement multithreading, we are gonna use a **handle_client** function in order for us to run a new thread for each client connection.

#### Updated code

**client.cpp:**
```
#include <iostream>
#include <cstring>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <thread>

void send_message(int sock) {
    std::string message = "Client message!";
    send(sock, message.c_str(), message.size(), 0);
    std::cout << "Message sent\n";
}

void receive_message(int sock) {
    char buffer[1024] = {0};
    int valread = read(sock, buffer, 1024); 
    if (valread > 0) {
        std::cout << "Server reply: " << buffer << "\n";
    }
}

int main(int argc, char *argv[]) {
    int sock = 0;
    struct sockaddr_in serv_addr;

    std::string server_ip = "127.0.0.1";
    int protocol = SOCK_STREAM; 
    if (argc > 1 && std::string(argv[1]) == "UDP") {
        protocol = SOCK_DGRAM; 
    }
    if (argc > 2) {
        server_ip = argv[2]; 
    }

    sock = socket(AF_INET, protocol, 0);
    if (sock < 0) {
        std::cerr << "Socket creation error\n";
        return -1;
    }

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(65432);

    if (inet_pton(AF_INET, server_ip.c_str(), &serv_addr.sin_addr) <= 0) {
        std::cerr << "Invalid address\n";
        return -1;
    }

    if (protocol == SOCK_STREAM) {
        if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
            std::cerr << "Connection failed\n";
            return -1;
        }
    }

    std::thread send_thread(send_message, sock);
    std::thread receive_thread(receive_message, sock);

    send_thread.join();
    receive_thread.join();

    close(sock);
    return 0;
}
```

**server.py:**
```
import socket
import sys
import threading

def handle_client(conn, addr):
    print(f"Connected by {addr}")
    data = conn.recv(1024)
    if data:
        print(f"Received: {data.decode()}")
        conn.sendall(b"Server message!")
    conn.close()

def start_server(host, port, protocol):
    with socket.socket(socket.AF_INET, protocol) as s:
        s.bind((host, port))
        if protocol == socket.SOCK_STREAM:
            s.listen()
            print(f"Server listening on {host}:{port} (TCP)...")
            while True:
                conn, addr = s.accept()
                # Create a new thread for each client connection
                client_thread = threading.Thread(target=handle_client, args=(conn, addr))
                client_thread.start()
        else:  # UDP
            print(f"Server listening on {host}:{port} (UDP)...")
            while True:
                data, addr = s.recvfrom(1024)
                print(f"Received: {data.decode()} from {addr}")
                s.sendto(b"Server message!", addr)

HOST = '0.0.0.0'
PORT = 65432
protocol = socket.SOCK_STREAM  

if len(sys.argv) > 1 and sys.argv[1] == 'UDP':
    protocol = socket.SOCK_DGRAM
if len(sys.argv) > 2:
    HOST = sys.argv[2] 

start_server(HOST, PORT, protocol)
```

#### New code running

<img src="/img/socket_3.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

<img src="/img/socket_4.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">


#### GUI

I`ve created a very simple GUI utilizing tkinter where u have 2 radials in order to choose between **TCP** and **UDP**, an IP field, a message field and a connect to server button.

#### Images:

<img src="/img/socket_5.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

<img src="/img/socket_6.png" alt="gif" style="display: block; margin-left: auto; margin-right: auto;">

All the code can also be found on my github at [Socket](https://github.com/AlexD2003/SocketUBB).