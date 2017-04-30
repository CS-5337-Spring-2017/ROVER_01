# CS 5337 - Advanced Software Engineering
## **Communication Server Documentation**
### Node.js & java


Spring 2017 <br/>
Computer Science Department <br/>
California State University, Los Angeles

**Description:** <br/>
Communication Server acts as a source of mediator between the Server and the Rovers, constantly transferring the required informaiton to the Rovers regarding the co-ordinates, locations of science on the map, messages and etc. All communications betwee the server and the rover are text based i.e., a series of Strings are shared between the Rover and the Communication Server in order to establish a successful connection between them. For the Communication Server to successfully communicate with the Rovers we need an API (Application Program Interface) to do that. We use these API's with a set of commands. 

There are a few RestFul API's that use HTTP requests to GET, PUT, POST and DELETE data which are discussed in brief below:

### **Global**

**[GET]** [/api/global/test](/api/global/test)

```
[{"x":1,"y":2,"terrain":"GRAVEL","science":"CRYSTAL"},{"x":3,"y":4,"terrain":"SAND","science":"ORGANIC"},{"x":5,"y":6,"terrain":"SOIL","science":"MINERAL"},{"x":7,"y":8,"terrain":"ROCK","science":"RADIOACTIVE"},{"x":9,"y":10,"terrain":"NONE","science":"NONE"}]

```
### **Sciences**

**[GET]** [/api/science/all](/api/science/all)

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
### **Gather**

**[POST]** [/api/gather/x/y](/api/gather/x/y)

example: /api/coord/82/18

```
{
  "x": 85,
  "y": 18,
  "science": "MINERAL",
  "terrain": "GRAVEL"
  "f": 17 // found by rover 17
}

```

**[POST]** [/api/coord/:x/:y/:science](/api/coord/:x/:y/:science)

```
header: 'Rover-Name' : rovername (ie. ROVER_14)
header: 'Corp-Secret': corp_secret
POST: /api/coord/30/40/DIAMOND

```
Now the coordinate looks like:

```
{
  "x": 30,
  "y": 40,
  "science": "DIAMOND",  // now updated to DIAMOND. (but should only use: NONE, CRYSTAL, ORGANIC, RADIOACTIVE, MINERAL)
  "terrain": "SOIL"
  "f": 14                // updated by rover 14
}
```
### **Misc**

**[GET]** [/api/rover/info](/api/rover/info)

```
{"ROVER_01":{"id":1,"sensor":"CRYSTAL","tool":"EXCAVATE","drive":"TREADS"}}

```
This class is the interface to the SwarmCommunicationServer nodejs or javscript program. It opens an HTTP Connection with the java class HttpURLConnection and sens a GET or POST request to the server in accordance with the server API

**[POST]** [/api/global](/api/global)

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

