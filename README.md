# **CS 5337 - Spring 2017 Advanced Software Engineering**
## **Group 1: Describe how the Communication Server works**
### Node.js & java


Computer Science Department <br/>
California State University, Los Angeles

**Description:** <br/>
Communication Server acts as a source of mediator between the Server and the Rovers, constantly transferring the required information to the Rovers regarding the co-ordinates, locations of science on the map, messages and etc. All communications between the server and the rover are text based i.e., a series of Strings are shared between the Rover and the Communication Server in order to establish a successful connection between them. For the Communication Server to successfully communicate with the Rovers we need an API (Application Program Interface) to do that. We use these API's with a set of commands. 

## **Communication Server**

The Communication Server acts as the link between the Rovers and the Rover Command Processor so that the rovers can retrieve and send information to the Command Processor. To be precise it is a restful Server that is implemented as a node.js application. It simulates a spacecraft that receives multiple requests from different rovers and also broadcast information based on the rovers request. The information that is sent through the communication server specifies the details about all the active rovers that are exploring the  
map on their respective terrain, It also includes the locations of all sciences that have been discovered and communicated by each rover to the communication server and the complete map showing what regions of the map have been explored and the locations which have not been explored yet. 
<br>

![initial scan](http://i.imgur.com/71sgoFe.jpg)
<br><br>

Once the rover starts exploring the map, it constantly sends the post request to the java communication in order to get the response from the Communication Server with the response of an array of JSON Objects. The rover keeps scanning the tiles and updates the information on what terrain or sciences have been located by the rover and keeps the Communication Server up to date with that information. This information will then be available to different rovers with the request and response messages which is a two way handshake process. All the objects that are located by each rover is also stored in the Communication Server as a JSON Object using the sendRoverDetailJSONDataToServer() method. These changes are also reflected in the console in the form of a basic UI as shown below:

![scanned locations](http://i.imgur.com/zd1kNv5.jpg)

<br>

## **What are the restful API interface commands?**

The restful API commands are pretty much the same commands that we use to send GET and POST requests to the server whenever the rover is either trying to retrieve some information from the server or push the data to the server. That is when we use these commands in the form of a URL and send a request to the server that is sent as a message to the server and the server responds back with a series of string type messages. Usually these messages are considered to be JSON Objects. Let us consider an example, one of the rovers on the planet is trying to get the map details to explore the map it send a GET request to the server using the [GET] [/api/global](/api/global) command in order to send a request to the server to get the next tile location on the map as shown in the picture above. Then the server responds by returning the global as a JSON array through which you can pull the required data from the JSON array. When it comes to sending information like science locations i.e., marking tile for gather etc, to the server then you send POST requests to the server using the [POST] [/api/gather/x/y](/api/gather/x/y) to send the details of the respective rover to the Communication Server. Here the 'x' & 'y' represent the rover coordinates on the map based on the X and Y axis of the map respectively. Again the restful API commands are categorized into different types, they are:


![restful API types](http://i.imgur.com/bWIr8D9.jpg)


Each of these types have their own set of commands in order to send and receive information through the java communications class which are listed below:

## Global
* [POST] [/api/global](/api/global)
    - it contains an array of JSON Objects sent by each active rover on the map or the planet which includes the coordinates of all the active rovers along with their terrain type, science locations, which rover found what type of science and which rover could gather what and returns everything as an array of JSON Objects in the following format:

```
[
  {
    "x":12,                 // coordinates must be Integers, not String
    "y":14,                 // ALL CAPS as in enums folder
    "terrain": "SAND",      // ROCK, SOIL, GRAVEL, SAND, NONE
    "science": "CRYSTAL",   // RADIOACTIVE, ORGANIC, MINERAL, CRYSTAL, NONE
    "f": 12                 // Found by Rover 12 (for debugging)
    "g": 15                 // Marked by Rover 15 for gathering
  }, ...
]
```

* [GET] [/api/global](/api/global)
    - It's just the same as the POST request but the only difference is in the POST method the information is given by the rover whereas with the GET request the rover is acquiring the information from the server that is the server is responding back to the rover by sending an array of JSON Objects for the rover to read the locations or updated science information on the map provided by other active rovers on the map.

* [GET] [/api/global/size](/api/global/size)
    - This API is most useful for testing purposes where the team can test the map boundaries for the planet so that the rovers don't keep wandering away from the specific location where the sciences are said to be located if you look at it as a real time based scenario.


* [GET] [/api/global/reset](/api/global/reset)
    - It is **highly** recommended to use this API only when you wish to reset the entire data on the map including the information sent by the rovers and the information that was already in-built in the initial stages.

The map that is sent or received by the rovers and the communication server is also available in a GUI format which looks something like this 

![map image](http://i.imgur.com/fMatJ4N.jpg)

As shown in the map above there are different types of terrains and science that is available throughout the map which we will discuss later.

## Science

* [GET] [/api/science/all](/api/science/all)
    - sdv

* [GET] [/api/science/drill](/api/science/drill)
    - Shows locations of sciences possible for driller

* [GET] [/api/science/excavate](/api/science/excavate)
    - Shows locations of sciences possible for excavater

## Gather
* [POST] [/api/gather/x/y](/api/gather/x/y)
    - Mark the tile that you are going to gather this science

## Coordinate
* [GET] [/api/coord/:x/:y](/api/coord/:x/:y)
    - Shows the coordinate at specified location

* [POST] [/api/coord/:x/:y/:science](/api/coord/:x/:y/:science) ** requires corp secret **
    - Update science information of this coordinate

## Miscellaneous
* [GET] /api/roverinfo
    - Shows information of the rovers

## **What is and how does the java communications interface class work?**
The java communications class is the interface to the SwarmCommunicationServer nodejs/javascipt program. There are three parameters that we use constantly sue in the communications class, their brief description is shown below:

 
Parameter | Description 
---------|----------
 url | used to define the link to the restful or interface API Commands 
 rovername | contains all the details about the specific rover 
 JSON Objects | contains all the details about that specific object (Ex: Rover Locations, Excavating Tools, Harvesting Tools, Destination sent by the other rover)

Since the communications between the rover and the server are all text based, whenever a rover sends a request to the server, the requests will be bi-directional, the server intermittently sends a response back to the rover with one or more lines of text. These responses will either be a simple text or can be objects that are encoded by json. The java communication class is responsible to send and retrieve information of the rover to the Communication Server. We use different methods to do so both to send information as well as retrieve the information.


## **_Methods used to send data to the Server_**
![sending details to the server](http://i.imgur.com/4eckWBs.jpg)

<br>

## **_Methods used to get the data from the Server_**
![getting details from the server](http://i.imgur.com/2LNK8ZK.jpg)



