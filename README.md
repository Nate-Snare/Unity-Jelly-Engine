# Jelly Physics
This jelly physics engine provides a framework that supports a variety of 2D games involving squishy objects.

## Classes
Classes include Level, Engine, Joint, Jelly, Vertex, Frame, and SubFrame. A visual hierarchy is as follows:

![alt text](<Jelly Physics Hierarchy.png>)

The major purposes, variables, and functions of these classes are reported below. These exclude basic properties handled by Unity, such as position and color.

### LEVEL
Stores level-specific commands, especially those based on user input.

### ENGINE
Manages the physics engine logic, including special input from Level objects. A container for Jelly and Joint objects.

#### Variables
* gravity
* jellies[]
* joints[]

#### Functions
* addJelly(jelly, position, angle=0)
* removeJelly(nameOrTag)
* addJoint(joint)
* removeJoint(nameOrTag)
* applyCollisions() - checks and corrects all collisions
* applyJoints() - applies all joints

### JOINT
Joints come in several types and function to hold jellies together, hold a jelly to a point on the screen, etc.

#### Variables
* name
* tags[]
* type

#### Functions
* void applyJoint() - applies the joint, generally by moving vertices' positions

### JELLY
Container for Vertex and Frame objects. Handles jelly-jelly collisions. A jelly may hold multiple frames and switch between them, effectively changing shape.

#### Variables
* name
* tags[]
* vertices[]
* frames[]
* currentFrame
* collisionTags[] - jellies may collide with one another only if they share a tag

#### Functions
* void toNextFrame() - sets currentFrame to the next frame in frames[]
* bool toFrame(str frameNameOrTag) - sets currentFrame to a frame of a certain name or tag in frames[]
* void fixCollision(Jelly otherJelly)

### VERTEX
Basic object that holds a position, mass, and velocity. Defines the edges of a Jelly object.

#### Variables
* mass
* velocity

#### Functions
void move()

### FRAME
Container for SubFrame objects.

#### Variables
* subFrames[]
* friction
* stiffness
* dampening
* gas - value in units of square area, representing the ideal area for the jelly, or -1
* liquid - value in units of square area, representing the exact area the jelly must have, or -1

#### Functions
* void applySubFrames() - applies all subFrames
* void applyFluids() - applies gas and/or liquid properties, if any

### SUBFRAME
References Vertex objects. Applies forces to vertices to help jellies maintain their shape.

#### Variables
* vertices[]
* springs[]
* originalAngles[] - the original angle from the COM for each vertex
* originalDistances[] - the original distance from the COM for each vertex
* centerOfMass - the current center of mass (COM)
* currentAngles[] - the current angles from the COM for each vertex
* offsetAngle - the average difference between the original and current angles
* targetPositions[] - the position of the original subFrame approximated on the current shape

#### Functions
* void applySubFrame() - applies forces to maintain jelly shape
* void applySpring(Vertex vertex, targetPosition) - applies a spring force to a vertex
* void getCenterOfMass()
* void getCurrentAngles()
* void getTargetPositions()