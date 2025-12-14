# deploying-lead-scoring-model-locally-with-kubernetes-and-use-autoscaling

Deploy locally lead scoring model with Docker, Kind and Kubernetes, add autoscaling option

## Buld docker image

Execute the following:

```bash
docker build -f Dockerfile_full -t zoomcamp-model:3.13.10-hw10 .
```

Run it to test that it's working locally:

```bash
docker run -it --rm -p 9696:9696 zoomcamp-model:3.13.10-hw10
```

And in another terminal, execute q6_test.py file:

uv run python q6_test.py

We should see this:

```json
{'conversion_probability': 0.49999999999842815, 'conversion': False}
```

Now we can stop the container running in Docker.

Next let's create a cluster with kind:

```bash
kind create cluster
```

And check with kubectl that it was successfully created:

```bash
kubectl cluster-info
```

To be able to use the docker image we previously created (zoomcamp-model:3.13.10-hw10), we need to register it with kind.

```bash
kind load docker-image zoomcamp-model:3.13.10-hw10
```

Then create deployment.yaml file and deploy it:

```bash
kubectl apply -f deployment.yaml
```

Check the deployment:

```bash
kubectl get deployments
kubectl get pods
kubectl describe deployment subscription
```

View logs:

```bash
kubectl logs -l app=subscription --tail=20
```

Now let's create a service for this deployment (service.yaml) and deploy it:

```bash
kubectl apply -f service.yaml
```

Check the service:

```bash
kubectl get services
kubectl describe service subscription
```

Testing the Deployed Service

We can test our service locally by forwarding the port 9696 on our computer to the port 80 on the service:

```bash
kubectl port-forward service/subscription 9696:80
```

Run q6_test.py once again to verify that everything is working. We should get the same result as before - roughly 0.49999999 ...

```bash
uv run q6_test.py
```

NB - we need to forward ports to redirect traffic - so we can run our tests as uv run q6_test.py ...
