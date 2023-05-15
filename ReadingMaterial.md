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



