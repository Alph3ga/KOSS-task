<div align="center">
<image src="https://upload.wikimedia.org/wikipedia/en/thumb/f/f4/Docker_logo.svg/180px-Docker_logo.svg.png">

# **Docker**

</div>

## Introduction to Docker :
<br>

### **What is Docker?**

Docker is a platform ( more specifically, a set of Platform as a Service products ) that allows you to host and deliver software in packages called containers, using OS level virtualization.

### **What is a container?** 

Simply put, it is a container ( as the name suggests ), that packages and provides everything your software needs to run. This isolates your software's behaviour from the rest of the system it is installed on.

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

To your process, it's running on the exact same computer each and every time.


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
4. Runs the command `go mod download`
5. Runs the command  `go build -o /bin/client ./cmd/client`
6. Runs the command  `go build -o /bin/server ./cmd/server`
7. Sets the command to start the executable 

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
<br>

## Relevant Features of the Linux Kernel :

Docker leverages several features that exist in the Linux Kernel to create containers. Two features that you need to understand to understand how the core features of Docker work, are namespaces and Cgroups.

### **Process :**

Firstly, what is a 'process'? A process is essentially an instance of the program, being run by the processor. It is a set of instructions, that the processor is following.

So how does a computer with say 8 processing units, simultaneously run hundreds of processes? By not actually running them simultaneously. 

A program in the Operating System, pauses runs a bit of one process, pauses it, runs a bit of another, pauses, and so on. This switching happens so fast and so many times per second, to us, it basically seems like all the processes are running simulaneously. All such processes have a process ID, or a `PID`. 

The Operating System decides how much processing time, or memory to give to each process. Intuitively, there is a problem in this system. Any process can demand more CPU resources, or start more child processes. Since resources are limited on any system, this affects other processes running on the system. 

To tackle this problem, Cgroups (Control Groups) are used.


`PID 1` is a special process `init`, that manages all other processes below it. Essentially, all processes exist in an heirachy, and have their own IDs. Any process can ask the Operating System for the list of processes running, or the network sockets, or the file system, and get the same information. They can also ask to communicate with any other process via protocols set by the Operating System. This creates a problem, especially for malicious programs.

To tackle this problem, Namespaces are used.

Another thing to address is how processes access memory. It would be near impossible (and insecure) for programmers to manually manage what memory belongs to them, and what memory belongs to other processes. So it is clearly a programming nightmare and a security hazard to let processes use actual physical memory addresses on the primary memory. 

Instead, processes use virtual memory addresses, which then the Operating System and the CPU map to the actual physical memory. Virtual memory addresses are contiguous, but the physical memory they are mapped to, doesn't have to be- the physical memory location doesn't even always have to exist on the primary memory.

This way, processes can only see their own memory, and believe they have that entire contiguous space available on the system.

<br>

### **CGroup :**

Control Groups are basically a way to **group** processes to **control** the resources they can access. Each control group has an associated controller, to control or limit the resources the processes within the group can access. 

This means that now processes can't take more memory than allotted to them and bring the entire system to a halt. I can group all processes of a type together in a control group, assign it 25% of the memory and 2 of the CPU cores, and now even if all those processes (within the group) come to a halt because one of them took too many resources, no other process is affected on the system.

A process can still know that it is in a control group, and see every other process on the system outside the cgroup, but if it isn't a root authorized process, it cannot do anything about it. 

<br>

### **Namespace :**

Namespaces are basically a way to lie to processes about the System and the environment they are running in. It is a way to segment and isolate groups of processes. 
One thing to note is that any child process will by default be in the same namespace as the parent process, unless explicitly moved to another namespace. No process can avail information restricted by its namespace.

Namespaces allow you to refer to different resources with the same name, the same resource with different names, and even not allow the process to refer to a resource at all (by not having a name in the namespace). Do can do this differently for all processes, making them feel they are in completely different systems.

There are different types of namespaces, to be briefly covered here. For more info, see the references at the bottom. 

**UTS Namespace :** Change the hostname of the system

**Mount Namespace :** Allows the processes within the namespace to view a selected part of the filesystem as if it were the entire filesystem. To it, the mounted part of the filesystem is everything that exists on the system.

**IPC Namespace :** Inter Process Communication namespaces restrict how processes communicate with each other.

**PID Namespace :** Seperates what processes a given process can see. Every PID namespace gives processes their own PID, starting with `PID 1`.

**Net Namespace :** Restricts what network resources (ports, IP address) can be accessed and used. Linux doesn't support putting physical network resources in any other namespace than the root namespace, so all communication is routed via virtual ports. To a process, there is no difference between a virtual and physical port.

**User Namespace :** Allows you to map any user inside the namespace to any actual user. This implies that within a user namespace, a process can have root control over all the processes inside it, but not outside. This allows processes to have root control over specific parts of the system, rather than the whole system.

---

<br>

## How does all this lead to a container?

Linux allows various overlapping combinations of Namespaces and Cgroups, but often having a varied combination can be messy and hard to manage. A common configuration is using all the namespaces and a cgroup, i.e., complete isolation from the system.

This configuration of using all the namespaces and a cgroup, is called a *container*. 

Each container has its own user structure, mounted filesystem, process IDs, virtual ports (mapped to other ports, physical or virtual), etc.

To a process(es) running inside this container, it is running on its own computer. It can use ports to connect to other containers, as if using physical ports to connect to another computer.

Another thing to note is the concept of read-only containers. Oftentimes, processes do not need to write to secondary memory. This allows us to bind (mount) the same filesystem containing the resources to even hundreds of different containers made using the same image. This makes containers extremely lightweight, and setting them up very fast. 

Docker is simply one of the tools made to automate the process of setting up these containers.

---

<br>

## The Docker Ecosystem :

Docker itself is a high level abstraction for running containers. Internally, it uses several other tools to actually access and manage the low level kernel features like cgroups and namespaces, and manage the creation of containers.

The `dockerd` sends this task to a program (daemon) called `containerd` (container daemon). The `containerd` program is a lower level container runtime that manages the lifecycle of a container, adding features like image distribution, setting up networks, etc.

Even `containerd` doesn't actually create and start a container, that is the job of the lowest level runtime `runc` to do that. `runc` contains `libcontainer`, a Go library for creating containers. It starts the container process and then exits.

Since `runc` exits, some program needs to manage the container. For this, `containerd-shim` is the daemon process that launches `runc`, which launches the container process. When `runc` exits, `containerd-shim` becomes the parent process of the container. 

What this allows is the redirection of the standard input output streams (`stdin`, `stdout`, `stderr`) according to our needs. It also allows `containerd-shim`, as the parent process, to know the exit code of the container process, and pass it on to `dockerd`.

---
## Additional references :
- [Dockerfile syntax reference](https://docs.docker.com/engine/reference/builder/)
- [Overview of common Namespaces](https://www.redhat.com/sysadmin/7-linux-namespaces)
- [Overview of Docker container runtimes](https://faun.pub/docker-containerd-standalone-runtimes-heres-what-you-should-know-b834ef155426)