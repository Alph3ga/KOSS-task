<div align="center">
<image src="https://upload.wikimedia.org/wikipedia/en/thumb/f/f4/Docker_logo.svg/180px-Docker_logo.svg.png">

# **Docker**

</div>

## Introduction to Docker :
<br>

### **What is Docker?**

Docker is a platform ( more specifically, a set of Platform as a Service products ) that allows you to host and deliver software in packages called containers, using OS level virtualization.

### **What is a container?** 

Simply put, it is a container ( as the name suggests ), that provides everything your software needs to run. This isolates your software's behaviour from the rest of the system it is installed on.

### **What is OS level virtualization?**

OS ( Operating System ) virtualization is when the Kernel ( the main part of the OS ) allows for the existence of multiple isolated 'areas' or 'compartments' within the same system. To a program running within such a compartment, the compartment looks like an entire real computer.

### **What do you mean 'everything'?**

'Everything' refers to first and foremost, the executable/script of your software. It also includes things like a runtime environment, frameworks, resources, dependencies, configuration data, environment variables, etc. To your software, all its needs are packaged neatly and exactly the same way every time.
<br><br>

---
<br>

## Need for Docker :
<br>

A typical software product needs a lot of things other than just the code you wrote/compiled. 

For example, a typical website may need:

- Node.js *(the runtime environment)*
- Express.js *(the backend framework)*
- React.js *(the frontend framework)*
- MongoDB *(database)*
- All the resource files for webpages
- Environment variables *(like access keys)*

It also needs to know how exactly you are going to interact with it, on what ports. 
<br><br>
Every time or everywhere you want to set up your website, you will have to repeat the same tedious steps of setting these things up exactly the way they need to be.

Any slip up- a file not being in the correct place, wrong version of some dependency being installed, some environment variable not being set up- might mean that your software doesn't run correctly, or doesn't run at all. Inevitably you end up saying *"But it works on my machine"*.
Cue hours and hours of trying to find out what went wrong.

<br>

### **In comes Docker**

<br>
Solution? While you are creating your software, just tell Docker how exactly to set things up. 

Now whenever and wherever you want to set up your project, you can do so in a fraction of the time it would have taken previously. You can deploy this container anywhere *(that has Docker installed)*, and it will work exactly the same everywhere.
<br><br>

---
<br>

## Docker Terminology :
<br>

### **Image :**
An 'Image' is basically a template using which Docker creates a container. It is read-only, i.e., an Image cannot be modified after creation. 

Images are made up of 'layers'. Each new layer is put on top of the other, adding some new modification.

A *'Dockerfile'* is a file that tells Docker how to build an Image. Using simple syntax, each line roughly translates into a single image layer.

<div align="center">
<img src="https://docs.docker.com/build/guide/images/layers.png" style="filter: invert(100%) hue-rotate(180deg);"/>
</div>

Here, to create the Image, Docker

1. Downloads an pre-built Image called '*golang:1.20-alpine*'
2. Sets the working directory to '/src'
3. Copies '. .' *(the directory containing the parent directory of the dockerfile)* to the working directory in the Image *(/src)*
4. Runs the command  `go build -o /bin/client ./cmd/client`
5. Runs the command  `go build -o /bin/server ./cmd/server`
6. Sets the command to start the executable 

The details of what exactly those commands do are not in the scope of this course *(refer to additional material at the end)*, what is important to understand here is how Docker builds an image step by step ( layer by layer ).

Docker can then make any number of containers using this same image as a template.
<br><br>

### **Container :**
A container is a runnable instance of an image. A container is defined by the image it is made from, but extra configuration might be provided.

Containers can be created, started, stopped, or moved easily. You can connect a container to a network, attach storage to it, or even create a new image from a running container.
<br><br>

### **Docker Daemon :**
'Daemon' is simply a program that runs as a background process, rather than an interactive process with the user. Commonly, Daemon programs are suffixed with 'd'.

Docker Daemon, or *'dockerd'* is the program that actually manages all images and containers behind the scenes.
<br><br>

### **Docker client :**
THis is the program that the user interacts with to use Docker. It is the connection between the client and the Docker Daemon, which it communicates with using the Docker API.

When you run a command like `docker build` or `docker run`, you communicate with the docker client, which sends the command to the docker daemon.


---
## Additional references :
- [Dockerfile syntax reference](https://docs.docker.com/engine/reference/builder/)

