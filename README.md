# day8
Creating something new...
Building a multi-threaded web server is a complex but rewarding project. Below is a high-level guide to creating a basic version of this project in C++. I will provide an outline of the steps and core components required. The actual implementation can be quite extensive, but this guide should give you a solid foundation to start with.

1. Setting Up the Project
Create a new C++ project in your preferred development environment or IDE. You can organize the project into multiple files for better structure (e.g., main.cpp, server.cpp, client_handler.cpp, logger.cpp, etc.).

2. Implementing Socket Programming
You will use the socket API provided by the operating system to handle network communication. Here’s a basic structure:
2.1. Include Necessary Headers
cpp
   CODE
#include <iostream>
#include <thread>
#include <vector>
#include <string>
#include <fstream>
#include <sstream>
#include <cstring>
#include <cstdlib>
#include <unistd.h>
#include <netinet/in.h>
#include <sys/socket.h>
2.2. Create a Socket
cpp
CODE
int create_server_socket(int port) {
    int server_fd;
    struct sockaddr_in address;
    int opt = 1;

    if ((server_fd = socket(AF_INET, SOCK_STREAM, 0)) == 0) {
        perror("Socket failed");
        exit(EXIT_FAILURE);
    }

    if (setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, &opt, sizeof(opt))) {
        perror("setsockopt");
        exit(EXIT_FAILURE);
    }

    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(port);

    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("Bind failed");
        exit(EXIT_FAILURE);
    }

    if (listen(server_fd, 10) < 0) {
        perror("Listen");
        exit(EXIT_FAILURE);
    }

    return server_fd;
}
