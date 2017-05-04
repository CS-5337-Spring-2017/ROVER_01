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


![initial scan](http://i.imgur.com/71sgoFe.jpg)

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

Once the rover starts exploring the map, it constantly sends the post request to the java communication in order to get the response from the Communication Server with the response of an array of JSON Objects. The rover keeps scanning the tiles and updates the information on what terrain or sciences have been located by the rover and keeps the Communication Server up to date with that information. This information will then be available to different rovers with the request and response messages which is a two way handshake process. All the objects that are located by each rover is also stored in the Communication Server as a JSON Object using the sendRoverDetailJSONDataToServer() method. These changes are also reflected in the console in the form of a basic UI as shown below:

![scanned locations](http://i.imgur.com/zd1kNv5.jpg)
