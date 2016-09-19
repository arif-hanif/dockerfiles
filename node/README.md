# Node
Running on Alpine Linux.

### Quick Start
`docker run --rm --name nodejs strues/node:6.6 node -v`

Using as Base Image
-------------------
If you want to use it as your base image you your `Dockerfile` should look like below

```sh
FROM strues/node:6.6

WORKDIR /app
ADD . .

# If you have to compile modules with node-gyp,
# you'll need extra tools. Uncomment the line below
# RUN apk add --no-cache make gcc g++ python

RUN npm install

# Replace app-port with your own one
EXPOSE [app-port]

# Replace app-entrypoint.js with your own one
CMD ["node", "app-entrypoint.js"]
```

Running your App
-----------------
To run your App run the following command given below (You need to change the path and command to reflect your app path)

```sh
$cd /path/to/project/root
#install dependencies
$docker run --rm --name awesome_node_app_install -v $(pwd):/app -w /app nmrony/alpine-node npm i
#Running App
$docker run --name awesome_node_app -v $(pwd):/app -w /app -p 80:<your-app-port> strues/node:6.6 node [app-entrypoint.js]
```
