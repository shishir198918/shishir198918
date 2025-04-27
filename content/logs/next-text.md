---
title: TCP Server
type: logs
prev: first-page
# next:
---
```python

from pprint import pprint
"""Provides Berkeley socket API originally written in C"""
import socket
import sys
import json
host="127.0.0.1"
port=1025
server_socket=socket.socket(socket.AF_INET,socket.SOCK_STREAM) # set  tcp socket
#print(socket.gethostbyaddr(host))
server_socket.bind((host,port))#server_socket.bind() #associating a socket with a specific network address (IP address and port number) so that it can receive incoming connections or send data to a specific destination

server_socket.listen(5) # backlog is optional and need to understand 
print(f"Server listening on {host}:{port}")
# synchronous server (take one request send response and then send anoher request)


def parse_header(header_binary):
    request_header={}
    header_lines=header_binary.strip().split(b"\n")
    for line in header_lines:
        #print(line)
        text_line=line.split(b":",1)
        request_header[text_line[0]]=text_line[1]
    return request_header    







def split_requestline_header_body(data):
    request={}
    rest_and_body=data.split(b"\n\n",1)
    if len(rest_and_body)==2:
        request["body"]=rest_and_body[1].decode()
    requestLine_and_header=rest_and_body[0].split(b"\n",1)
    request["requestline"]=requestLine_and_header[0].decode()
    if len(requestLine_and_header)==2:    
        request["header"]=requestLine_and_header[1].decode()
    return request


def synchronous_handling():
    try:
        while True:
            clientsocket, address = server_socket.accept() # reciving incoming connection and blocking untill        
            print(f"Connected to {address}")
            
            string=b""
            try:
                
                while True:
                    data=clientsocket.recv(1024)
                    print(f"chunk---->{data}",end="\n\n")
                    string=string+data
                    
                    if data.endswith(b"\n"): #see tcp checksum 
                        request=split_requestline_header_body(string)
                        break                   
                clientsocket.sendall(json.dumps(request).encode())
            except BrokenPipeError:
                print("client connection is close")
                clientsocket.close()
                break
    except KeyboardInterrupt as e:
        print(f"Server closing: {e}")
        server_socket.close()
        sys.exit(1)

                       
def echo_server():
    try:
        while True:
            clientsocket, address = server_socket.accept() # reciving incoming connection and blocking untill        
            print(f"Connected to {address}")
            
            while True:
                string=b""
                try:
                    data=clientsocket.recv(1024)

                    clientsocket.sendall(json.dumps(request).encode())
                except BrokenPipeError:
                    print("client close is connection")
                    clientsocket.close()
                    break
    except KeyboardInterrupt as e:
        print(f"Server closing: {e}")
        server_socket.close()
        sys.exit(0)
           

#synchronous_handling()
#parse_header(sys.argv[1])




def parse_http(data):
    #paring the request 
    pass

def line_sep(text):
    pass

header="""User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br, zstd
Referer: https://www.google.com/
Connection: keep-alive
Cookie: prov=bd3331a1-0e17-4400-8c2d-225d7ee182b7; __gads=ID=38a9745c5807bd26:T=1732272308:RT=1743656236:S=ALNI_MZxCWewJk-X6awkW3-e6Ja3poABPw; __gpi=UID=00000f725da6c535:T=1732272308:RT=1743656236:S=ALNI_MbuhNKrts2JfWDU-iR5DI15Mu6Zpw; __eoi=ID=f1f2c8b3db2cbabc:T=1732272308:RT=1743656236:S=AA-AfjaZdJ0ioqfdq3ggvicKQ9o5; OptanonConsent=isGpcEnabled=0&datestamp=Thu+Apr+03+2025+10%3A27%3A16+GMT%2B0530+(India+Standard+Time)&version=202411.2.0&browserGpcFlag=0&isIABGlobal=false&hosts=&consentId=6ea3cba6-2464-4af9-a552-fc5b4cd8ae49&interactionCount=1&isAnonUser=1&landingPath=NotLandingPage&groups=C0001%3A1%2CC0002%3A1%2CC0003%3A1%2CC0004%3A1&intType=1&geolocation=IN%3BKA&AwaitingReconsent=false; g_state={"i_p":1744823880343,"i_l":4}; OptanonAlertBoxClosed=2024-11-22T10:45:14.231Z; _so_tgt=8f2a8479-96df-4fa1-938d-6479364f1bd1; _ga=GA1.2.738961413.1732274815; _ga_WCZ03SZFCQ=GS1.1.1743655267.110.1.1743656235.60.0.0; __cflb=02DiuFA7zZL3enAQJD3AX8ZzvyzLcaG7w1j8Paardd8SY; _gid=GA1.2.2106724934.1743508671; cf_clearance=mahwwospHq3eqKKKwIQwkQn7HtxYTHZ8tsYxkL4Kceo-1743656235-1.2.1.1-L8utDyLvR6PbUnaaQTeNQMWl6FDff.wl.7c2Svpmp9cc13whv6LxdThelH8Dx.wjVJRMkXbWorFRBGnqtbJSDdlU1QID8P13rKpJjbl8lIpx7c6oCSRZAZmrbkUF0Uu0tshM7LBChdpB2S3ffA_XUIq3C1DB4wEQB4ZtSNl26NVynDmiv4MtAbjL1lVUBaQuWcSh2DrXQWu0n9cPbnTYupsrfMzxFZfEh42UBQpBBqeZgUGojENR8dBz.GuDJWM4pWY1IeareOesKC2YU1wc.1EChQbgrW2qeIF6nChkvVVShoCEbUvk_qnFpl1nv_m.0r7esmsBx0qgapRC_RzC0sOX.WV9JtyqO4Q2z_l8S.8; _cfuvid=lhxXGQdm4hrFv.pKx2C7HBcDEKlbt0mkcMBKrvTLIzY-1743656235379-0.0.1.1-604800000; __cf_bm=KJI2IzIDP1dwJGybbukc0fwbO0qG7hJwJuorXafEr3A-1743656235-1.0.1.1-tnnsLH8BSuzUQjrHsnw2t7qOhXBPEznU5B4L5mJwTfiX_b9GaObhqxv8JB7rXCDqc218Dr2D7rGeNqkubKSBlRwLshJJWufD.bVpQwGXRWE
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Priority: u=0, i"""


header1="""Host: httpbin.org
Connection: close
Accept: */*
User-Agent: Mozilla/4.0 (compatible; esp8266 Lua; Windows NT 5.1)
Content-Type: application/json
"""
pprint(parse_header(header1.encode()))


```