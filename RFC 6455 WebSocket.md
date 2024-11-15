Intended to establish a connection with a server of pre-existing protocols such as SMTP and HTTP.

Intended to fail when data from other protocols is sent to WS server.
- Fields such as |Sec-| cannot be set by an attacker from we web browser using only HTML and JS APIs
#### TCP AND HTTP
Independent TCP-based protocol
- Only relationship to HTTP is the handshake is interpreted by an HTTP server as an Upgrade Request.
- Uses port 80 by default and 443 for WS connection tunneled over Transport Layer Security (TLS)
- Connection over 443 are significantly more likely to succeed

#### Establishing Connections
When a connection is made to a port shared by an HTTP server (likely to occur using port 80 and 443), it will appear as a `GET` request with an `Upgrade` offer.
- One IP address and single server allows for practical deployment.
- Elaborate set-ups (load balancers and multi-server), dedicated hosts for WS connections, separate from HTTP servers, is most likely easiest to manage.

#### Subprotocols
Client can opt-in via |Sec-WebSocket-Protocol| field in handshake.
- Server must respond with same field of one of the selected subprotocols.
- Versioning supported, backward-incompatible
	- e.g. `chat.example.com` and `v2.chat.example.com` would be considered completely separate WS clients.
	- reusing existing subprotocol string and handling subprotocol different would be necessary.

#### WS URIs
This specification defines two URI schemes, using the ABNF syntax defined in RFC 5234 [RFC5234], and terminology and ABNF productions defined by the URI specification RFC 3986 [RFC3986].

    ws-URI = "ws:" "//" host [ ":" port ] path [ "?" query ]
    wss-URI = "wss:" "//" host [ ":" port ] path [ "?" query ]

    host = <host, defined in [RFC3986], Section 3.2.2>
    port = <port, defined in [RFC3986], Section 3.2.3>
    path = <path-abempty, defined in [RFC3986], Section 3.3>
    query = <query, defined in [RFC3986], Section 3.4>

The port component is OPTIONAL; the default for "ws" is port 80, while the default for "wss" is port 443.

#### Client Requirements
/host/, /port/, /resource name/ and /secure/ flag must be passed along with /protocols/ and /extensions/ to be used.
- Requires /origin/ if client is web browser.

Must send an opening handshake field and read the server's response.

Not focusing on this as much as I am only implementing the server functionality and using the WebSocket JavaScript API to implement the client-side functionality.

[Section 4: Opening Handshake](https://www.rfc-editor.org/rfc/rfc6455#section-1.5)

## Data Framing
#### Overview
Data is transmitted in frames. 

Server __MUST NOT__ mask frames sent to client
Server __MUST__ close connection upon receiving a frame that is unmasked.
- MAY send Close frame with a 1002 (protocol error) code.

Clients __MUST__ mask frames being sent to server because:
1. Avoid confusing network intermediaries (such as proxies)
2. Security reasons
Client __MUST___ close connection upon receiving a frame that is masked from server.
- MAY send Close frame with a 1002 (protocol error) code.

Frame types composed of:
- Opcode
- Payload Length
- Specific locations for "Extension data" and "Application data"
	- Together define "Payload data"

#### Base Framing Protocol
Authoritative wireframe for data transfer
![[Pasted image 20241114212826.png]]
