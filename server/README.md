# LocalSend Signaling Server

A signaling server for LocalSend. Using Rust and WebSockets.

## Docker build

```bash
git clone https://github.com/localsend/localsend
cd localsend
docker build -f server/Dockerfile -t localsend-signaling-server:latest .
```
