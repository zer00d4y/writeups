# W1seGuy Write-up

Task file `source-1705339805281.py`

    import random
    import socketserver 
    import socket, os
    import string
    
    flag = open('flag.txt','r').read().strip()
    
    def send_message(server, message):
        enc = message.encode()
        server.send(enc)
    
    def setup(server, key):
        flag = 'THM{thisisafakeflag}' 
        xored = ""
    
        for i in range(0,len(flag)):
            xored += chr(ord(flag[i]) ^ ord(key[i%len(key)]))
    
        hex_encoded = xored.encode().hex()
        return hex_encoded
    
    def start(server):
        res = ''.join(random.choices(string.ascii_letters + string.digits, k=5))
        key = str(res)
        hex_encoded = setup(server, key)
        send_message(server, "This XOR encoded text has flag 1: " + hex_encoded + "\n")
        
        send_message(server,"What is the encryption key? ")
        key_answer = server.recv(4096).decode().strip()
    
        try:
            if key_answer == key:
                send_message(server, "Congrats! That is the correct key! Here is flag 2: " + flag + "\n")
                server.close()
            else:
                send_message(server, 'Close but no cigar' + "\n")
                server.close()
        except:
            send_message(server, "Something went wrong. Please try again. :)\n")
            server.close()
    
    class RequestHandler(socketserver.BaseRequestHandler):
        def handle(self):
            start(self.request)
    
    if __name__ == '__main__':
        socketserver.ThreadingTCPServer.allow_reuse_address = True
        server = socketserver.ThreadingTCPServer(('0.0.0.0', 1337), RequestHandler)
        server.serve_forever()
