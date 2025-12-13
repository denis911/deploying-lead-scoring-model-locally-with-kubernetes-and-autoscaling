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
{'conversion_probability': <value>, 'conversion': False}
```
