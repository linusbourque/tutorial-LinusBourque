<h1>Deploy Applications with Kind (MacOS)</h1>

Creating containers for application development is a fairly easy task but when you need to create 1000s or more, something more robust is needed. That’s where Kubernetes (or sometimes referred to as K8s) comes in. Kubernetes is a popular container orchestration platform that is used by many organizations to deploy applications. But if you’re just starting, deploying 1000s of containers may be overkill. Maybe you just need 2-3 for a simple web application. You could still manage these manually individually. Or you can use [kind](https://kind.sigs.k8s.io/) to gain practice with Kubernetes on our local systems. Let's deploy an application with **kind** to see how it works. To begin, start by installing kind from https://kind.sigs.k8s.io/. 

<H2>Prerequisites</H2>  

 *  [docker](https://docs.docker.com/desktop/)
 *  [go](https://go.dev/) 1.16+


<H2>Install kind</H2>
## [Sub-concept 1]
Depending on your local system OS, the installation instructions are straightforward. For MacOS, the instructions will vary between Intel chipsets or M1/M2/M3 chipsets. 

For Intel chipsets use the following command in a terminal.

`[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-darwin-amd64`


For Silicon chips (M1/M2/M3) use the following command in a terminal.

`[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-arm64`

Note: the command defines the architecture (`uname-m`) and makes sure it matches the architecture version being pulled (`= x86_64`). The double ampersand (`&&`) says continue to the next command if the first worked. So if the architecture matches, then do the curl command. cURL (which stands for Client for URL) will pull and install *kind*. The flags, `-Lo`, allows for a location to be set as to where you want **kind** installed. In the example above, kind will be installed in the directory where the command was run. 

![Screenshot of installing kind on a Silicon Mac and it’s install location](https://thevirtualbuddha9.wordpress.com/wp-content/uploads/2025/02/screenshot-2025-02-23-at-12.48.25e280afpm-copy.png)

With **kind** installed, there are two last steps to complete.

1.  Enable the execute bit on the kind application: `chmod +x ./kind`
2.  Move the executable to your preferred location for installed binaries. For example: `sudo mv ./kind /usr/local/bin/kind`

<h2>Starting kind with bootstrap Cluster</h2>
## [Sub-concept 2]

Once **kind** is installed, you will need to create a bootstrap Kubernetes cluster. This uses a pre-built node image. We return back to our terminal with the following command:

`./kind create cluster`

If successful, you should see the output below:

![Screenshot of the kind control pane being built](https://thevirtualbuddha9.wordpress.com/wp-content/uploads/2025/02/screenshot-2025-02-23-at-12.48.05e280afpm.png)

You can also open Docker Desktop to see the control pane running.

![Screenshot of Docker Desktop that shows kind-control-pane cluster running](https://thevirtualbuddha9.wordpress.com/wp-content/uploads/2025/02/screenshot-2025-02-23-at-5.01.14e280afpm.png)

For those that prefer the command line to verify whether kind is running correctly, you can use `kubectl cluster-info --context kind-kind`. ![Screenshot of command kubectl cluster-info --context kind-kind](https://thevirtualbuddha9.wordpress.com/wp-content/uploads/2025/02/screenshot-2025-02-23-at-8.05.55e280afpm.png)

Note: If, however, you get the error below, it means that Docker is not installed. This is easily resolved by going to [Docker](https://www.docker.com/) and installing Docker Desktop. Once Docker Desktop is installed, re-run the command.
![Screenshot of kind failing to build the cluster, with an error showing that the $PATH does not have the docker executable in it](https://thevirtualbuddha9.wordpress.com/wp-content/uploads/2025/02/screenshot-2025-02-23-at-12.48.44e280afpm.png) 



# [Concept 2]

`#ffffff`
```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: web
  name: web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: web
    spec:
      containers:
      - image: gcr.io/google-samples/hello-app:1.0
        name: hello-app
        resources: {}
status: {}
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: web
  name: web
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: web
  type: NodePort
status:
  loadBalancer: {}
```
`#000000`


## [Sub-concept 2]

# Cleanup

# Next Steps
