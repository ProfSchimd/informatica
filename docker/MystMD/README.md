Commands
```sh
# Build the image
docker build -t mystmd-python .
# Run the container
docker run -it --rm \
  -p 3000:3000 \
  -v ~/myst-project:/workspace \
  mystmd-python
```
