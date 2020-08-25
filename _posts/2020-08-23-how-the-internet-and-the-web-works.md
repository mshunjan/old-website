--- 
title: 'How the Internet and the Web Works'
date: 2020-08-23 14:58 -0400
category: computer science 
--- 

# Table of Contents
- [Why the Basics Matter](#why-the-basics-matter)
  * [Resources](#resources)
  * [Intro](#intro)
- [The Internet and The Web - A Romance for the Ages](#the-internet-and-the-web---a-romance-for-the-ages)
  * [The Internet](#the-internet)
    + [A Few Internet Connection Methods](#a-few-internet-connection-methods)
      - [Wireless](#wireless)
      - [Mobile](#mobile)
      - [Cable/Broadband](#cable-broadband)
  * [Finding Your Computer](#finding-your-computer)
  * [The Web](#the-web)
  * [HTTP](#http)
  * [HTTP METHODS](#http-methods)
  * [HTTP Headers](#http-headers)
  * [Response Codes](#response-codes)

# Why the Basics Matter

## Resources

Watch this [video](https://www.youtube.com/watch?v=uEmF74eHRO8). It pretty much covers everything in here and much more. 

## Intro
Before you read any of this know this. You're either stuck in tutorial hell, or you've come back to brush up on skipped subjects. If you're the former please realize you'll never make something until you do just that. Think to yourself what can I make? Maybe a web app that creates a survery based on people's favorite foods. Maybe you can make a design app like GIMP. If you know enough of a language to pretty much do anything, nothing fancy but you have the knowledge to build most things, then you're ready. The thing with developing is that you're constantly learning. It's a headache the first-time building an application, but once you attempt it yourself, you gain an understanding  you could never have simply reading tutorials and following through them.  

That being said, I've been pondering for a while what exactly created the barrier between becoming a good developer and a basic understanding of how to code things. Undoubedtly, it has to do with a crucial understanding of the components that make up the bigger system. There's this idea that people know how to code, but they have no idea what's going on. Magic code they call it. Although that may seem fun (in some sense), this can greatly hinder your progress, even if you manage to make some projects work. In that sense we're going to look at Web Development from this point of view, and in plain english.

To begin such a dive, it's important to understand the difference between the *Internet* and the *Web*. Often used interchangeably, they are actually not the same. The Internet represents a network of networks spanning the entire world. The Web is however the collective information that is accessible through the aforementioned network of networks. So we use the Internet Infrastructure to access the service Web. An interesting analogy is that of a bus. The various bustops can be though of the Internet, whereas the bus is the Web. 

# The Internet and The Web - A Romance for the Ages
## The Internet

Let's assume things like wifi don't exist for this example. If you have a computer and you want to connect to another computer, at the most basic level, you can connect them using a cable. Now they can talk to each other. To picture this, image two computers connected by a single cable. Now what if I want multiple computers? We add more cables. Now image your computer has 10 different cables coming out of it, connected to 10 different computers, and each of those 10 computers is doing the same thing. We would have 100 cables, just for 10 computers!

It's very hard to scale this up so we need some sort of central hub. A place we're all the computers connect to. We can do this with a mini-computer called a router. Now instead of 100 cables for our network of 10 computers, we now only need 10. The router acts like a traffic light that tells computers how to and when to send messages to each other. You can think of this setup, as you'll see below, as our network.

![Computer Network]("https://cyburp.com/mshunjan.github.io/assets/images/computer-network.jpg")

So if we want to scale this up further, we could just connect our router to another router. In this manner we can get to a really large number of computers connected together. But this is only for our network.

Now we need to connect to other people's network. To continue our cable example, we would need to connect everybodies routers to each other through some sort of connection. Luckily things such as telephone wires, radio and TV cables already exist. To hop onto these sorts of connections, we use devices called modems. This device converts the digital data it recieves from a router to a signal that can pass through telephone wires or radio. Tv cables are special in that they themselves are a modem. 

Finally, those signals going through telephone wires, radio or tv cables are sent to an Internet Service Provider (ISP), which are companies  that have a special network of networks. The ISP can then connect to other ISP's and what we're left with is a giant network connecting everyone. 

Below we go over the internet connection methods if you're interested, but here's a recap of what we know:

- We create a network of computers using cables and a router
- Routers control communication between computers on a network
- We then connect our router to a modem to hop onto existing infrastructure such as telephone wires
- This reaches the ISP which can send your message to the destination network


### A Few Internet Connection Methods

#### Wireless

For wireless connections radio is used, where your house's router would be connected to a modem. Also, hybrids exist, where your modem is also a router. Regardless, as long as your within a certain area, your connected to the internet.

#### Mobile
How does your phone connect to the internet, when there's no wifi. You use the cellular network! Basically the network consists of areas in the world called cells which contain some sort of signal transciever. Imagine these "cells" spread across your city, province or country. As long as your phone falls within the radio coverage, your phone can connect.

#### Cable/Broadband
You've probably heard of this before. Note that Cable is a type of Broadband. Essentilly you connect through tv cable lines, which act as a sort of modem.

## Finding Your Computer

We talked about sending messages but we didn't say how we know who's message is who's. We need a way to uniquely identify a network connection. 

When a computer is linked to a network, each computer is assigned an IP (Internet Protocol) address. We can also alias this address with domain names, such as google.com, so we don't have to remember an address to find a website.  We can think of an IP address as someone's nickname. Think of it like your mailing address. 

## The Web
So the web is all the info accessible through the internet. Let's take a look at what happens when you open a web-page on your device.

The internet largely consists of client and server computers. Client computers are going to be your computer, phone. The server computers actually store the webpages, sites and apps you look at. At the most basic level, the client copies the desired webpage from the server and displays it in a web browser. 

When you look up a website, your browser goes to a Domain Name Server (DNS), and matches the website name you put into the browser to a specific IP address. Remember that the IP address identifies a computer. Next your browser sends an HTTP request message to the server, where it asks the server "can you send me a copy of this website". HTTP just refers to a protocol for clients and servers to talk to each other. So this HTTP request is sent across the internet via another protocol called TCP/IP. TCP defines how to establish and maintain a network converstation. IP defines how computers send data. Assuming the server agrees to send a copy of the website, it sends the files for the website to the browser in chuncks called packets. Then the browser assembles those packets for you into a web page. Why use packets? It makes moving data much easier and scaleable. 

## HTTP

As I said, HTTP/ Hypertext Transfer Protocol is way for computers on the web to talk to each other. But where is this exactly? The communication model for talking on the web works in 4 layers:

1. Network access(link) -> describes access to media
2. Internet(IP) -> describes how data is packaged
3. Transport(TCP/UDP) -> describes how data is delivered
4. Application(HTTP) -> describes meaning of messages

This is how the current internet works but lets look at an older model which is still used to describe how these layers work. The OSI model has 7 layers:

1. Physical (e.g. cable, RJ45)
2. Data Link (e.g. MAC, switches)
3. Network (e.g. IP, routers)
4. Transport (e.g. TCP, UDP, port numbers)
5. Session (e.g. Syn/Ack)
6. Presentation (e.g. encryption, ASCII, PNG, MIDI)
7. Application (e.g. SNMP, HTTP, FTP)

Don't worry to much about the details. All we care is what each layer does. So we'll start with layer 7. This is where you are right now,looking at the web browser. This layer send data for doing something such as showing a picture or words to layer 6. It essentially translates data to machine code so the computer can understand it. Furthermore, security protocols happen here such as encryption and decryption. Next we have layer 5, which decides which data packets belong to which files and it is where communication starts and begins between computers. Layer 4 recieves those data packets and handle transport. Data recieved from 5 is segmented and each segment recieves a source, a destination port and a sequence number. The port makes sure segment reaches the write application. Sequence ensures the data comes in the write order (remember data has to be re-assembled).  this layer also controls ammount of data transmitted and error-checking. Reaching Layer 3 is the transmission of packets between networks with travel points marked by IP addresses. Layer 2 is the data link, where actual physical addressing occurs. This layer deal with mac addresses, which are the actual unique, physical addresses of your devices. It's like a serial number.

## HTTP METHODS
I think this example really sits in the idea of how a client communicates with the server.

```bash
telnet www.examplewebsite.com 80
GET / HTTP/1.1
Host: www.examplewebsite.com
```
First we connect to the server with examplewebsite's files. Next we use a GET request, which means we want to get some data. Why do we put the number 80? It is the port number from which your computer uses HTTP.

We specify which page using /, meaning the root and then we tell which HTTP version we are using.The final command tells the computer which site we want to reach. 

First we have URLs, which identfies where things are found on a website. When you start making and interacting with API's several HTTP methods will begin to be important. These are: OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT. 

**GET** - This method request a representation of the specified resource.

**HEAD** - This method produces headers for a GET request. So instead of the actual data, you might get meta-data.

**POST** - This method lets us send data to the server.

**PUT** - This method is like post but put can be called multiple times with no different effects. Post on the other hand can have diff. effects.

**DELETE** - This method deletes a resource.

**TRACE** - This method performs a message loop-back to the target. Pretty much how we debug. 

**CONNECT** - This method  pretty much opens a communication channel.

## HTTP Headers
Basically these are a way to pass in extra info during an HTTP request/response. A header looks something like this `name:value`. Its kind of like a python dictionary in that sense. They really serve a purpose in terms of security, in that we can prevent people from exploiting vulnerabilities, or we could say hacking.

## Response Codes

This is basically the way the server can tell us what happend with the request. 

200 - What you requested worked! 

307 - What you requested has been temporarily moved somewhere else

301 - Address changed

410 - The info on this URL is gone

500 - Unknown why request didn't work. Could be a number of issues