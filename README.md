# 2a_Stop_and_Wait_Protocol
## AIM 
To write a python program to perform stop and wait protocol
## ALGORITHM
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
```
import socket
import threading
import time

# Server function
def server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_socket.bind(('localhost', 12345))

    print("Server is listening on port 12345\n")

    for i in range(5):
        data, client_address = server_socket.recvfrom(1024)

        print("Server received:", data.decode())

        # Send ACK
        server_socket.sendto(b"ACK", client_address)
        print("Server sent: ACK\n")

    server_socket.close()


# Client function
def client(frame_size):
    time.sleep(1)  # Wait for server to start

    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    server_address = ('localhost', 12345)

    for i in range(5):

        prefix = f"Frame {i + 1}: "

        if frame_size < len(prefix):
            print("Frame size must be at least", len(prefix))
            return

        # Create frame of required size
        frame = prefix + 'X' * (frame_size - len(prefix))

        print("Client sending:", frame)

        # Send frame
        client_socket.sendto(frame.encode(), server_address)

        # Wait for ACK
        ack, _ = client_socket.recvfrom(1024)

        print("Client received:", ack.decode())
        print("Stop and Wait...\n")

        time.sleep(1)

    client_socket.close()


# Main program
frame_size = int(input("Enter the frame size: "))

# Create server and client threads
server_thread = threading.Thread(target=server)
client_thread = threading.Thread(target=client, args=(frame_size,))

# Start both
server_thread.start()
client_thread.start()

# Wait for both to finish
server_thread.join()
client_thread.join()

print("Transmission completed successfully!")

```

## OUTPUT

<img width="654" height="287" alt="Screenshot 2026-08-21 143841" src="https://github.com/user-attachments/assets/3936ebca-99f0-4058-9a1d-ce190c03db61" />


## RESULT
Thus, python program to perform stop and wait protocol was successfully executed.
