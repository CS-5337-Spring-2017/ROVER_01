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
    - This api is used from the java communication class to the server in order to retrieve all the science locations from the server which also includes the type of terrain on which the science is on. These locations are constantly updated by the rovers whenever they find a new science that is located on the map and is pushed to the server using that specific API request command. The overview of all the objects that are included in the array of JSON Objects is shown below:

```
    [
        {
            "x": 19,
            "y": 47,
            "science": "ORGANIC",
            "terrain": "SOIL",
            "f": 13,  // found by rover 13
            "g": 18   // marked by rover 18 for gather
       } ...
    ]

```

Each active rover on the map is allocated two different types of tools. One is the scanner where as the other could either be a drill or an excavator. The first one is used to scan the map tiles to find whether there exists any science at that location or not and the latter one is used to harvest that specific tile where the science is located but this is limited only to the rover that can both harvest that tile as well as has the necessary tools to scan the science on that location. There are different types of sciences available all over the map which will be discussed going forward 

* [GET] [/api/science/drill](/api/science/drill)
    -  This API specifies all the locations of sciences that is possible for the driller to extract. For example, If you have a rover that is a WALKER and has a drill as one of its tools and there's a science on a ROCK Terrain, when you use this API command the locations are popped up on the map so that the rover can go to that location to drill and gather the science out of that terrain. 

* [GET] [/api/science/excavate](/api/science/excavate)
    -  This API specifies all the locations of sciences that is possible for the excavator to extract. For example, If you have a rover that is WHEELS and has an excavator as one of its tools and there's a science on a SOIL or GRAVEL Terrain, when you use this API command the locations are popped up on the map so that the rover can go to that location to excavate and gather the science out of that terrain.

## Gather
* [POST] [/api/gather/x/y](/api/gather/x/y)
    - When the rover is exploring the map and it finds that there's some science next to its locations then it send this information to the server and the server sends a broadcast message to all the other rovers. The rover that is most close to that location will mark that tile as gathering tile by this API by including the rover coordinates based on the a and y axis points of the map. When a rover points the gathering update to a specific location then that specific tile details look like this which includes Header which consists of the ROVER_Name and the Corp-Secret value and the post request with its coordinate value as shown below

```
        header: 'Rover-Name' : rovername (ie. ROVER_11)
        header: 'Corp-Secret': corp_secret
        POST: /api/gather/78/52

        Now the tile looks like this:
        {
                "x":78,
                "y":52,
                "terrain": "SAND",      // ROCK, SOIL, GRAVEL, SAND, NONE
                "science": "CRYSTAL",   // RADIOACTIVE, ORGANIC, MINERAL, CRYSTAL, NONE
                "f": 10                 // Found by Rover 10 (for debugging)
                "g": 11                 // Marked by Rover 11 for gathering   
        }

```


## Coordinate
* [GET] [/api/coord/:x/:y](/api/coord/:x/:y)
    - As explained in GATHER API when a rover finds some science but cannot gather it, it sends a GET request to the server using the x and y coordinate locations so that the other rover who is close by can locate it and if possible gather it. 

* [POST] [/api/coord/:x/:y/:science](/api/coord/:x/:y/:science) 
    - This API is pretty much the same as the previous one except for that these locations are forwarded by the rovers to the communication server notifying that it has found some science that it is visible to it and updates the science location on the map for the same. These updates can also be seen by the other rovers

## Miscellaneous
* [GET] [/api/roverinfo](/api/roverinfo)
    - Each and every minute detail about the rover is stored in this API that is all the rover information including the name, id, sensor, tools and the drive system of the rover is stored in this API. 

## **What is and how does the java communications interface class work?**
The java communications class is the interface to the SwarmCommunicationServer nodejs/javascipt program. There are three parameters that we use constantly sue in the communications class, their brief description is shown below:

 
Parameter | Description 
---------|----------
 url | used to define the link to the restful or interface API Commands 
 rovername | contains all the details about the specific rover 
 JSON Objects | contains all the details about that specific object (Ex: Rover Locations, Excavating Tools, Harvesting Tools, Destination sent by the other rover)

Since the communications between the rover and the server are all text based, whenever a rover sends a request to the server, the requests will be bi-directional, the server intermittently sends a response back to the rover with one or more lines of text. These responses will either be a simple text or can be objects that are encoded by json. The java communication class is responsible to send and retrieve information of the rover to the Communication Server. We use different methods to do so both to send information as well as retrieve the information.


## **_Methods used to send data to the Server_**
![sending details to the server](http://i.imgur.com/BSbw8zT.jpg)

<br>

## **_Methods used to get the data from the Server_**
![getting details from the server](http://i.imgur.com/2LNK8ZK.jpg)

All the above specified restful API commands are used in the java communications and the javascript file for the server to send responses back to the requests made by the rover to retrieve the information to make the necessary changes like moving to the specified destination by other rover, gathering sciences on a specific location of the map, drilling or excavating science from the terrain, finding status of other rovers on gathering sciences. All these requests and responses are mostly relied on the GET and POST methods in the Communication Class. 


## **What are some recommendations for different ways in which the rovers can negotiate duties such as mapping or harvesting?**
In order to increase the potential of the program it is best to use the "Divide and Rule" policy where each rover is assigned a different job to do. Let us say we have 3 rovers (walkers, wheels and treads) roaming around the planet. Instead of the rovers being all over the place we can distribute different duties to each rover like the wheels are the fastest rovers on the map so it will be advantageous for it to explore the map and update different locations of the map constantly. Doing so the other rovers can focus on which place they want to go and since not all rovers can move over all types of terrains, the treads which are a little slower than the wheels can go over the terrains Sand, Soil and Gravel whereas the walkers which are the slowest of all the rovers can travel over Soil, Gravel and Rocks but get stuck upon entering the Sand Terrain and the wheels get stuck as soon as they enter the sand terrain so it is best for the Treads rover to find the type of surrounding that are on different locations of the map. Once we have the map locations with what type of terrain is there at which location each of the rovers can the do their own will of harvesting sciences. But, how do they do it when each of the rover is travelling in a different location and what would be the worst case scenario where the rover is in a location where it sees something but it cannot extract it. This is when we can use the API's to communicate with the server and MAP the location to the server to communicate with the other rovers saying "Hey I found something over here, it's open for Harvesting". 

The image below shows that ROVER_02 has found something but can't harvest it and changes the tiles status to open for harvest.
![found something](http://i.imgur.com/Y8Q164T.jpg)

Other rovers let us take ROVER_05 being the closest is trailing upon a different science location since it's status is pending so it sends a message to the Communication Server that it is already going to get a science from a different location.

![pending to harvest something else on the map](http://i.imgur.com/WENrwR0.jpg)

Then ROVER_01 being the next closest ROVER to the science location sends a message to the Communication Server saying hey I'm open for gathering and then changes it's status from open to pending harvest with the tile coordinates as the destination and highlights the path to it. 

![open for gathering science](http://i.imgur.com/8r6HCWe.jpg)

Doing so, any rover that is close to that location can change the status of going to that location and harvesting the tile to pending. So, each map tile that has some sort of science will have some sort of data which also gives information about the status of the tile as explained below.

<p align="center"><img src="http://i.imgur.com/EYcns25.jpg"></p>

As shown above status has three different definitions namely open, pending and done which is basically given as the parameters to a map tile which adds an additional JSON object called status. Each value of this JSON Object defines a different meaning to the status value which is shown below:

![status_description](http://i.imgur.com/u7E5e6B.jpg)

Now in the future version if there is any hidden trap and the rover has fallen into this trap then the rover sends a message to the server with a new JSON object which can be a boolean value which changes to true if stuck else it remains falls. If all the active rovers in the map are given the capacity to have a towing tool then we can apply the same logic that we have used for mapping the status of the tile for gather logic to the rover towing when stuck in a hidden trap that the rover is not aware of. For us to do that in a much simple way where the complexity of the logic is as minimum as possible we can use the highlighted path logic for the rovers current location to the destination where the rover is stuck because this highlighted path cannot pass through the obstacles until and the unless the rover has the tendency to go over that terrain type. Once the rover reaches the other rover that is trapped in the hidden trap then it can use the towing logic to pull the rover out of that trap. Below is the example on how the JSON values look like for the hidden trap and trapped rovers that need to be towed out. 

```
[
  {
      rs: 4                 // Rover Stuck is 4
      trapped: true,        // Rover is trapped? yes
      x: 27,                // x coordinate
      y: 8,                 // y coordinate
      terrain: "HIDDEN",    // terrain type
      tb: 3                 // towed by Rover 3
  }
]
```

![stuck on hidden trap](http://i.imgur.com/OdnMkb6.jpg)


## **How can additional commands and functions be added to the Communications Server and accessed or utilized by the rovers?**

* Instead of applying different logics by the rovers to calculate the distance between the rovers and the sciences on the map for gathering sciences, it will be really helpful if such function is available from the server side to calculate the distance between all the rovers and the science that has been found by some other rover. 

* The highlighted path logic can be directly implementing the highlighted path logic into the server with necessary parameters passed to the function that are color coordinated to each different rover will give a clear user interface to the viewer as to which rover is going towards which direction or tile location. This function will also reduce the chances of rovers colliding with each other. 

* Declaring API commands in app.js and create a HTTP Connection on rover side to call those commands. For example let us consider the following code to be written in the communication class

```
readScienceDetailJSONDataFromServer(){
    ....
     obj = new URL(url + "/science/all/");
    HttpURLConnection con = (HttpURLConnection) obj.openConnection();
    ....
}

```
and use the following commands in javascript

```
 app.get('/api/science/:option', function (req, res)  

```
Doing so we can directly create an object in the communication class and call its method to get the location of all the sciences in the map

```
(ScienceDetail[] scienceDetails = communication
               .getAllScienceDetails();)

```
![Highlighted path](http://i.imgur.com/sF4OGsN.jpg)

* The above shown image is as of now only limited to a single rover since all the rovers were to create their own RoverCommandProcessor initially. So if this kind of logic is directly used in the RoverCommandProcessor for all the rovers many loopholes can be covered like running into rovers or getting stuck in an unknown land or even bumping into walls if the rover logic is implemented the correct way.
 

## **Make some recommendations on how to improve the implementation of the project. Make some recommendations on additional features and functions to add to the simulation such as, liquid terrain features, hex vs. square map tiles, power limitations (solar, battery, etc.), towing, chance of break downs, etc.**

Few of the recommendations for the implementations for better functionality of the project will include:

* A central server which has the tendency to locate all kinds of sciences and terrain will be helpful to send the data to the rovers which will also save time for scanning each and every tile on the map for exploring purposes.

* 