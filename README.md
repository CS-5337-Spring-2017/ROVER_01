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
| [controlServer](src/controlServer) | class for control UI display also Rover behavior |
| [enums](src/enums) | enums classes for constant such as Science type, Terrain type etc. |
| [json](src/json) | classes for JSON data conversion for communication with server |
| [rover_logic](src/rover_logic) | classes for various rover algorithms |
| [swarmBots](src/swarmBots) | classes for each Rover instance|
