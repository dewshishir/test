import socket

def run_client():
    try:
        # Create a socket object
        client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        server_ip = "127.0.0.1"
        port = 8000
        
        # Try connecting to the server
        client.connect((server_ip, port))
        print(f"Connected to server {server_ip}:{port}")
        
        while True:
            # Send message to the server
            message = input("Enter message to send (type 'close' to disconnect): ")
            client.send(message.encode("UTF-8"))  # Convert string to bytes
            
            # If the user sends 'close', break out of the loop before receiving a response
            if message.lower() == "close":
                break
            
            # Receive response from the server
            response = client.recv(1024)  # Receive up to 1024 bytes
            response = response.decode("UTF-8")  # Convert bytes to string
            print(f"Server response: {response}")
        
        # Close the connection
        client.close()
        print("Disconnected from server")
    
    except ConnectionRefusedError:
        print("No server available to connect to. Please start the server first.")
    except Exception as e:
        print(f"An error occurred: {e}")
    finally:
        # Ensure socket is closed if an error occurs
        try:
            client.close()
        except NameError:
            pass

# Corrected block for script entry point
if __name__ == "__main__":
    run_client()
