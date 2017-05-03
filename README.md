# Rover Swarm Project - Rover 01

Rover swarm project is a swarm of autonomous robot simulator. It provides a virtual map populated by robots and sensors, along with various "sciences" for the robots to sense and gather.
The map that rovers will be exploring will consist of a map that contains a selection of terrain that will include Rocks, Soil, Gravel, and Sand. Spaced throughout the map will be assorted “Science” to be found and harvested. The Science can consist of Radioactive, Crystal, Mineral, or Organic samples.

## Requirement Software
below here are minimum list of software that require to run this project
* Windows 7 or compatible operating system
* [Eclipse](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/keplersr1) - IDE for JAVA developer
* [node.js](https://nodejs.org/en/download/) - for running rovers communication server
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) - version control system, git provides easy way to clone the project
* This project requires at least JDK 1.8.0_77

## Major Classes
Major classes of this project are list here.

| Package | Description |
| ------ | ------ |
| [common](src/common) | common classes for object such as Rover, Map, Science etc.<br>restful API interface for communication with server |
| [controlServer](src/controlServer) | class for control UI display and Rover behavior |
| [enums](src/enums) | enums classes for constant such as Science type, Terrain type etc. |
| [json](src/json) | classes for JSON data conversion for communication with server |
| [rover_logic](src/rover_logic) | classes for various rover algorithms |
| [swarmBots](src/swarmBots) | classes for each Rover instance|
  
## Rover Swarm Simulation
The simulation of rover swarm is consist of 3 major components.
1. Communication Server - REST API server used as means to exchange information with Rovers
2. Rover Command Processor (RCP) - control map and UI display also update the map based on command that recieved from rovers
3. Rovers - contain programs for map exploration and gathering sciences

### Communication
The communication between each components are:
1. RCP and Rovers - Socket is used for communication between RCP and Rovers. RCP runs on a specific computer and has a socket that is bound to a specific port number 9537. The server just waits, listening to the socket for Rovers to make a connection request. On the client-side:  The ROVER_XX program will connect to the server on ‘localhost’ ‘port 9537’ as the default. The ROVER_XX program will accept an IP address as a runtime attribute. This will be the address that the rover program will connect to the server instead of localhost should the program have to be run over a network. It will still use port 9537 as the default. ROVER_XX send requests for the services they require to the RCP, and the RCP responds accordingly. Threads are using to  handling these requests. RCP creates a thread for every request it receives from ROVER_XX. The thread is a set of instructions which run differently from the program or other such threads. Using this, RCP can easily multitask well. It can start a thread for a Rover and then continue communicating with other Rovers.
