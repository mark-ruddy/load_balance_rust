# Load Balance Rust
Load balanced configuration for a Rust Axum server serving on port 80 on a K3s cluster which uses Traefik.

1. If this is a single-node K3s cluster(K3s master node runs the workloads), then to free up ports 80 and 443 change the K3s traefik controller: `sudo cp traefik-config.yaml /var/lib/rancher/k3s/server/manifests && sudo systemctl restart k3s`

2. Start the K3s cluster and ensure a registry is running: `sudo systemctl start k3s && k apply -f cfg.yaml`

3. Build the image: `cd server && podman build . -t localhost:30000/axum-server:0.0.1 && podman push localhost:30000/axum-server:0.0.1`

4. Create the Axum server deployment and its load balancer: `k apply -f deploy.yaml && k apply -f bal.yaml`

5. Server should be available and exposed on the host: `curl http://localhost:80`
