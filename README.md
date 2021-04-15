# Fanout Platform Helm Charts

This is the official Helm Charts repository for installing Fanout Platform on Kubernetes. Note that the charts depend on private Docker images. Use the contact form at [Fanout Platform](https://fanout.io/enterprise/) to get access before using the charts.

## Install

Add the Helm repository:

```sh
helm repo add fanout https://charts.fanout.io
helm repo update
```

Make a yaml file with Docker Hub credentials:

```yaml
imageCredentials:
  username: alice
  password: 12345
```

Install the package:

```sh
helm install -f creds.yaml --generate-name fanout/fanout
```

## Testing it out

Set up port forwarding:

```sh
# get the release name
export NAME=`helm list --short | grep fanout`

# run each of these in separate terminals
kubectl port-forward service/$NAME-conn-proxy 7999:80
kubectl port-forward service/$NAME-conn-api 5561:80
```

Connect a client:

```sh
curl -i http://localhost:7999/stream
```

Publish a message:

```sh
curl \
  -H "Content-Type: application/json" \
  -d '{"items":[{"channel":"test","formats":{"http-stream":{"content":"hello world\n"}}}]}' \
  http://localhost:5561/publish/
```
