Custom Python Netcat
This project provides a simple, custom implementation of the classic Netcat utility written in Python. It allows for basic network operations such as establishing TCP connections, listening for incoming connections, executing commands remotely, and transferring files.

Features
Client Mode: Connects to a specified target host and port to send/receive data.

Listener Mode: Sets up a server to listen for incoming connections on a specified port.

Command Shell: Provides an interactive remote command shell for connected clients.

File Upload: Allows clients to upload files to the listening server.

Command Execution: Executes a specified command on the listener upon connection.

Graceful Shutdown: Handles Ctrl+C (KeyboardInterrupt) for a clean exit.

Dependencies
This script uses only standard Python 3 libraries. No external packages need to be installed via pip or other package managers.

The following Python modules are used:

sys

socket

getopt

threading

subprocess

signal

time

Setup and How to Run
Save the Code:
Save the entire provided Python code into a file named netcat.py.

# Example: Create the file
touch netcat.py
# Then paste the code into netcat.py using your preferred editor

Ensure Python is Installed:
Make sure you have a Python 3 interpreter installed on your system.

Linux/macOS: Python is usually pre-installed. Verify with python3 --version.

Windows: Download and install Python from the official website: https://www.python.org/downloads/

Run the Script:
Open your terminal or command prompt and navigate to the directory where you saved netcat.py.

Command Line Options
SYNOPSIS
    python netcat.py [OPTIONS...]

OPTIONS
    -l, --listen            Start server in listening mode on specified host:port
    -e, --execute=<file>    Execute specified file upon connection establishment
    -c, --command           Initialize an interactive command shell session
    -u, --upload=<path>     Upload file to specified destination path on connection
    -t, --target=<host>     Specify target hostname or IP address
    -p, --port=<port>       Specify target port number

Usage Examples
1. Listener Mode (Command Shell)
This sets up a server that listens for incoming connections and provides an interactive command shell to the connected client.

On the Listener (Server) Machine:

python netcat.py -l -p 9999 -c

Output on Listener:

[*] Listening on 0.0.0.0:9999

On the Client Machine:

python netcat.py -t 127.0.0.1 -p 9999

(Replace 127.0.0.1 with the Listener's IP address if connecting remotely)

The client will then see a prompt like <Target:#> and can type commands.
Example on Client:

<Target:#> whoami

Output on Client:

your_username
<Target:#>

2. Listener Mode (File Upload)
This sets up a server to receive a file from a client and save it to a specified destination.

On the Listener (Server) Machine:

python netcat.py -l -p 8888 -u received_file.txt

Output on Listener:

[*] Listening on 0.0.0.0:8888

On the Client Machine (to send my_document.txt):

# Read the file content and pipe it to the netcat client
cat my_document.txt | python netcat.py -t 127.0.0.1 -p 8888

Output on Listener after upload:

Successfully saved file to received_file.txt

3. Listener Mode (Execute Command on Connection)
This sets up a server that, upon receiving a connection, immediately executes a specified command and sends its output back to the client.

On the Listener (Server) Machine:

# Execute 'ls -la' when a client connects
python netcat.py -l -p 7777 -e "ls -la"

Output on Listener:

[*] Listening on 0.0.0.0:7777

On the Client Machine:

python netcat.py -t 127.0.0.1 -p 7777

The client will immediately receive the output of ls -la and then the connection will close.

4. Client Mode (Simple Data Transfer)
This allows you to send data from standard input to a remote server.

On the Server (e.g., using a native Netcat listener):

# Using native netcat on Linux/macOS
nc -l -p 6666

Or use this Python script as a simple listener:

python netcat.py -l -p 6666

On the Client Machine (to send a message):

echo "Hello from the Python client!" | python netcat.py -t 127.0.0.1 -p 6666

The server will receive "Hello from the Python client!".

Important Considerations
Permissions: If you try to listen on a port number less than 1024 (e.g., 80, 443), you will likely need elevated privileges (e.g., run with sudo on Linux/macOS or as Administrator on Windows).

Firewall: Ensure your system's firewall allows connections on the ports you are using. You might need to add rules to permit traffic.

Security: This is a basic demonstration of network programming principles. It lacks encryption and robust authentication. Do not use this script for transferring sensitive data over untrusted networks or in production environments. For secure communication, always use tools that implement strong encryption (like SSH or TLS/SSL).

Error Handling: The script includes basic error handling for common network issues, but it's not exhaustive.
