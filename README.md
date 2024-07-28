# LiveKit distributed setup with k8s

K8s infrastructure to test LiveKit required ports/protocols for a distributed setup, experimenting with the various options :

- range for UDP WebRTC host candidates
- all traffic in one ICE/TCP
- all traffic in one ICE/UDP Mux


Observations:

- For a distributed setup, each node has to be able to connect to redis
- LiveKit can work only with the API/WS port + ICE/TCP or ICE/UDP. Nodes can be isolated (blocking all the traffic but those ports + they need to be able to access redis)
- Nodes communicate and discover themselves only through redis
   - Audio packages are relayed through redis pub/sub on the signal>relay topics. 
   - When connecting with Client1 to Node1 and Client2 to Node2 to the same room (no matter which node hosts it), there seems to be no additional traffic/communication between Node1 and Node2 a part from redis, they just talk to their clients.
- If I create Room1 and join it through Node1, connecting with another client to the same room through Node2 will still keep the normal connection to Node2, no additional connections or redirections are created. Node2 will act as a proxy and relay "transparently".
   - Clients connected to Node1 do not need to be able to reach Node2 to talk with clients connected through Node2 or to join rooms “hosted” on Node2.
 

See: https://docs.livekit.io/home/self-hosting/ports-firewall/

