# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
## client
```
import socket

def send_request(host, port, request):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((host, port))
    s.sendall(request)
    response = s.recv(4096).decode()
    s.close()
    return response

def upload_file(host, port, filename):
    file = open(filename, "rb")
    file_data = file.read()
    file.close()

    content_length = len(file_data)

    headers = "POST /upload HTTP/1.1\r\n"
    headers += "Host: " + host + "\r\n"
    headers += "Content-Length: " + str(content_length) + "\r\n\r\n"

    request = headers.encode() + file_data

    response = send_request(host, port, request)
    return response

host = "localhost"
port = 8080
filename = "example.txt"

response = upload_file(host, port, filename)

print(response)
```
## server
```
import socket

server = socket.socket()
server.bind(("localhost", 8080))
server.listen(1)

print("Server waiting for connection...")

conn, addr = server.accept()
print("Connected by:", addr)

while True:
    data = conn.recv(4096)
    if not data:
        break

    print("\nReceived from client:\n")
    print(data.decode(errors="ignore"))

    response = "HTTP/1.1 200 OK\r\n\r\nFile received successfully"
    conn.send(response.encode())

conn.close()
server.close()
```
## OUTPUT
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/726f317d-2e24-426b-9e9a-174807e38fbe" />
<img width="1918" height="1078" alt="image" src="https://github.com/user-attachments/assets/5d93375c-ef9e-4a6f-b90b-dbe6ca59af46" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
