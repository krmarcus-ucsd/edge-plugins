# edge-plugins/plugin-smokedetect

Docker container usage
-------------
The docker image is hosted on [sagecontinuum](https://hub.docker.com/orgs/sagecontinuum).

Build the image:
```bash
docker build --build-arg TOKEN=${TOKEN} -t sagecontinuum/plugin-smokedetect:0.1.0 .
```
where `TOKEN` is set enviroment varibale and created through [JWT](https://jwt.io/) with the appropriate secret.

Run the container:
```bash
docker run sagecontinuum/plugin-smokedetect:0.1.0
```
# Instructions
The following instrutions are to serve a user from start to finish how to create the smoke detection plugin.

## Step 1: clone beehive repository 
```bash
git clone https://github.com/waggle-sensor/beehive-server
```
Start the server:
```bash
cd beehive-server
./do.sh deploy
```

## Step 2: run trainning jupyter notebook and save model
First clone the smoke detection model:
```bash
git clone https://gitlab.nautilus.optiputer.net/i3perez/keras-smoke-detection
```
Create a kubernetes deployment (on Nautilus):
```bash
kubectl create -f kerasDeloyment.yaml
```
Run the juputer notebook as describe in the README file. At the end of the notebook
the trainned model will be save to the object storage through [SAGE REST API](https://github.com/sagecontinuum/sage-restapi)

## Step 3: clone waggle repository
```bash
git clone https://github.com/waggle-sensor/waggle-node
```
To start the waggle node (this assumes that there is an existing beehive instance already created):
```bash
./waggle-node up
```

To start a plugin you will need to create a docker image and pass the following command:
```bash
./waggle-node schedule waggle/plugin-smokedetect:0.1.0
```
