Install the project:
--------------------
1. Start NetBeans IDE
2. File -> Import -> From ZIP... -> Select "KspPathPlanning.zip"

Initialization file:
--------------------
Initialization file name is "init.txt". It is located in a folder "initialization".
Double click on the file name to open it.
Initialization file contains the values of following constants:
* K - number of feasible paths to find (the default value is MAX_POSSIBLE_VALUE)
* MAX_PATH_LENGTH - maximum length of feasible paths (the default value is 100)
* FIND_SIMPLE_PATHS - set to true to find only paths without cycles (the default value is true) 
* SHOW_GRAPH - set to true to see connectivity model (the default value is FALSE)
* SHOW_PATHS - set to true to see feasible paths (the default value is TRUE)
* START_NODE - name of the start node (default value is not set)
* TARGET_NODE - name of the target node (default value is not set)
* CONNECTIVITY_MODELS - space separated list with file names that contains 
  connectivity models (default value is not set) 

Connectivity models:
--------------------
Connectivity models are described using text files that are located in the "connectivity" folder.
Each file contains four sections: NODES, LINKS, NODES_TO_REMOVE and LINKS_TO_REMOVE.
In Section NODES, for each node, the names of all nodes adjacent to it are described.
In Section LINKS, for each node, the names of the links that connect the node to its adjacent nodes are described.
Section NODES_TO_REMOVE contains a list of node names that need to be removed.
Section LINKS_TO_REMOVE contains a list of link names that need to be blocked.

---------------------------
Example 1 (Figure 2 and 3):
---------------------------
1. Find all feasible paths from a node "room-3312" to a node "room-3405".
Connectivity model: base connectivity level 

K: 1000
START_NODE: room-3312  
TARGET_NODE: room-3405
CONNECTIVITY_MODELS: building-base.txt 

2. Find all feasible paths from a node "room-3312" to a node "room-3405".
Connectivity model: merge connectivity models (departments and vertical connectivity)

K: 1000
START_NODE: room-3312  
TARGET_NODE: room-3405
CONNECTIVITY_MODELS: building-dep1.txt building-dep2.txt building-vc.txt

3. Find all feasible paths from a node "room-3312" to a node "room-3405".
Connectivity model: merge connectivity models (floors and vertical connectivity)

K: 1000
START_NODE: room-3312  
TARGET_NODE: room-3405
CONNECTIVITY_MODELS: building-floor3.txt building-floor4.txt building-vc.txt

Result: 4 feasible paths  
  
-----------------------------------------------------
Example 2 (pathfinding on complete undirected graphs):
-----------------------------------------------------

Find all feasible paths between two nodes in an undirected complete graph 
with n=10 nodes.  Change n to find other results.
Connectivity model: base level

SHOW_PATHS: false
START_NODE: room1  
TARGET_NODE: room2
CONNECTIVITY_MODELS: graph_n10.txt

Result: 109601 feasible paths
  

-------------------------------------
Example 3 (Intra-storey pathfinding):
-------------------------------------

Scenario 1a. Find all feasible paths from a node "room-3304" to a node "room-3316"  
Connectivity model: base hierarchy level

START_NODE: room-3304  
TARGET_NODE: room-3316
CONNECTIVITY_MODELS: rectorate-base.txt

Scenario 1b. Find all feasible paths from a node "room-3304" to a node "room-3316"  
Connectivity model: connectivity model for 3rd floor 

START_NODE: room-3304  
TARGET_NODE: room-3316
CONNECTIVITY_MODELS: rectorate-f3.txt

Result: 1 feasible path


Scenario 2a. Find all feasible paths from a node "room-3304" to a node "elevator"  
Connectivity model: base hierarchy level

START_NODE: room-3304  
TARGET_NODE: elevator
CONNECTIVITY_MODELS: rectorate-base.txt

Scenario 2b. Find all feasible paths from a node "room-3304" to a node "elevator"  
Connectivity model: merge connectivity model for 3rd floor and vertical connectivity

START_NODE: room-3304  
TARGET_NODE: elevator
CONNECTIVITY_MODELS: rectorate-f3.txt rectorate-vconnectivity.txt

Result: 9 feasible paths

Scenario 3a. Find all feasible paths from a node "room-3312" to all washrooms on 3rd and 4th floors 
Connectivity model: base hierarchy level  

START_NODE: room-3312  
TARGET_NODE: regex(^WC.*f3)
CONNECTIVITY_MODELS: rectorate-base.txt

Scenario 3b. Find all feasible paths from a node "room-3312" to all washrooms on 3rd and 4th floors 
Connectivity model: merge connectivity model for 3rd floor, 4th floor, and vertical connectivity   

START_NODE: room-3312  
TARGET_NODE: regex(^WC.*f3)
CONNECTIVITY_MODELS: rectorate-f3.txt rectorate-f4.txt rectorate-vconnectivity.txt

Result: 4 feasible paths


-------------------------------------
Example 4 (Inter-storey pathfinding):
-------------------------------------

Scenario 1a. Find all feasible paths from a node "room-3414" to a node "printing"
Connectivity model: base hierarchy level

SHOW_PATHS: false
START_NODE: room-3414 
TARGET_NODE: printing
CONNECTIVITY_MODELS: rectorate-base.txt

Scenario 1b. Find all feasible paths from a node "room-3414" to a node "printing"
Connectivity model: merge connectivity model for basement (level B1F), 4th floor, and vertical connectivity

SHOW_PATHS: false
START_NODE: room-3414 
TARGET_NODE: printing
CONNECTIVITY_MODELS: rectorate-b1f.txt rectorate-f4.txt rectorate-vconnectivity.txt

Result: 75 feasible paths

Scenario 2a. Find all feasible paths from a node "room-3304" to all men's rooms 
on the 3rd and 4th floors
Connectivity model: base hierarchy level

SHOW_PATHS: false
START_NODE: room-3304 
TARGET_NODE: regex(^WC-men.*f(3|4))
CONNECTIVITY_MODELS: rectorate-base.txt

Scenario 2b. Find all feasible paths from a node "room-3304" to all men's rooms 
on the 3rd and 4th floors
Connectivity model: merge connectivity model for 3rd floor, 4th floor, and vertical connectivity

SHOW_PATHS: false
START_NODE: room-3304 
TARGET_NODE: regex(^WC-men.*f(3|4))
CONNECTIVITY_MODELS: rectorate-f3.txt rectorate-f4.txt rectorate-vconnectivity.txt

Result: 10 feasible paths


---------------------------------------
Example 5 (Inter-building pathfinding):
---------------------------------------

Scenario 1a. Find all feasible paths from node "room-3414" to node "depository-4-f3"
Connectivity model: base hierarchy level for "Rectorate" and "Library" buildings

SHOW_PATHS: false
START_NODE: room-3414
TARGET_NODE: library.depository-4-f3
CONNECTIVITY_MODELS: rectorate-base.txt library-base.txt

Scenario 1b. Find all feasible paths from node "room-3414" to node "depository-4-f3"
Connectivity model: merge connectivity model for "Rectorate" 4th floor, 
"Library" 3rd floor, and vertical connectivity for both buildings

SHOW_PATHS: false
START_NODE: room-3414
TARGET_NODE: library.depository-4-f3
CONNECTIVITY_MODELS: rectorate-f4.txt library-f3.txt rectorate-vconnectivity.txt library-vconnectivity.txt

Result: 5400 feasible paths


Scenario 2a. Find all feasible paths from node "room-3414" to outdoors
Connectivity model: base hierarchy level for "Rectorate" building

SHOW_PATHS: false
START_NODE: room-3414
TARGET_NODE: outdoors
CONNECTIVITY_MODELS: rectorate-base.txt

Scenario 2b. Find all feasible paths from node "room-3414" to outdoors
Connectivity model: merge connectivity model for 4th floor and 
vertical connectivity 

SHOW_PATHS: false
START_NODE: room-3414
TARGET_NODE: outdoors
CONNECTIVITY_MODELS: rectorate-f4.txt rectorate-vconnectivity.txt

Result: 75 feasible paths

Scenario 2c. Find all feasible paths from outdoors to a node "depository-4-f3" 
Connectivity model: base hierarchy level for "Library" building

SHOW_PATHS: false
START_NODE: outdoors
TARGET_NODE: library.depository-4-f3
CONNECTIVITY_MODELS: library-base.txt

Scenario 2d. Find all feasible paths from outdoors to a node "depository-4-f3" 
Connectivity model: merge connectivity model for 3rd floor and 
vertical connectivity 

SHOW_PATHS: false
START_NODE: outdoors
TARGET_NODE: library.depository-4-f3
CONNECTIVITY_MODELS: library-f3.txt library-vconnectivity.txt

Result: 72 feasible paths




Other examples:
===============  
  
Example 1: Evacuation from "Rectorate" building
Find all feasible paths from the multiple starting nodes to outdoors.
Connectivity model: base hierarchy level

K: 1000
MAX_PATH_LENGTH: 11
SHOW_PATHS: false						
START_NODE: preprint warehouse room-3004 rector rector-secretary vice-rector-1 vice-rector-2 WC-women-f2 dean-faculty-EE dean-faculty-M room-3114 registry WC-men-f1 cashier room-3304 room-3312 room-3305 room-AIUT room-3309 room-3402 room-3403A room-3414 room-3412 WC-men-f4 
TARGET_NODE: outdoors
CONNECTIVITY_MODELS: rectorate-nodes.txt 

Result: 804 feasible paths

To disable "elevator" node add this name in section NODES_TO_REMOVE in rectorate-nodes.txt 
Result: 246 feasible paths

		