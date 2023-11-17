# Moovee
Moovee enables you to form complex and structured animations using relatively high-level descriptions. It works on an intricate events-based system to give control of every component of the animation, should you choose to make specific configurations.

## Terminology
Moovee uses film-making terminology to provide highly intuitive and readable scripting. The library that allows for versatile, complex rates-of-change (`Actions`), which can be attached to renderable objects (`Actors`) and then grouped into synchornised animations (`Scenes`), all aggregated together with user configurations to be played back as a `Movie`.

## Example
The following code glides a box smoothly across the screen (symmetrical quadratic derivative) while spinning at an increasing rate (linear derivative):
```c++
// create a camera
Camera camera = {800, 800};

// define fundamental animation components
Vec2 pFrom   = {100, 100},  // starting position
     pTo     = {300, 500},  // ending position
     pMinMax = {0, 10};     // min and max speeds

Vec2 aFrom   = {0, 0},      // starting angle
     aTo     = {PI / 2, 0}, // ending angle
     aMinMax = {0, 0.1};    // min and max angular velocities

// initialise scripts
Script* pScript = new Script(Actions::symQuad, {pFrom, pTo, pMinMax}, MV_TRANSLATE); // script for position
Script* aScript = new Script(Actions::linear, {aFrom, aTo, aMinMax}, MV_ROTATE);     // script for angle

// create an actor
std::vector<Vec2> box = {
  {10, 10},
  {20, 10},
  {20, 20},
  {10, 20}
};
Actor* actor = new Actor(box, {pScript, aScript});

// create scene for a synchronised animation that reaches the final state in 1000 iterations
Scene* scene = new Scene({actor}, 1000);

// after each iteration, render
scene.afterTick = [](std::vector<Actor*> actors) -> bool {
     camera.renderActors(actors);
     return true;
};

// play scene
scene.play();

```

## Brief Documentation
A full documentation is in the works. The following should help you get started.

### Camera
The camera is an SDL window and renderer with additional methods for rendering and transformations.
The camera has members:
* `viewport`
* `zoom`
* `origin`
* `offset`
* various all-purpose rendering methods

### Scripts
A script is essentially a geometric transformation.
The constructor function takes in as parameters:
* A gradient function (returns Vec2)
* Behaviour specifications (called directions)
* A script type (MV_TRANSLATE, MV_ROTATE, MV_SCALE)
Scripts contain multiple event callbacks that can be overridden. Returns a bool, indicating whether to dent or permit permisson to play the script.

### Actors
An actor is a set of 2D vectors to which scripts are applied.
The constructor function takes in as parameters:
* A set of Vec2 points
* A set of scripts
Actor possess their own event callbacks that can be overidden. Returns a bool, indicating whether to deny or permit permission to play all scripts applied to the actor.
The centre of an actor's geometry is its centroid by default and is automatically updated. The value of the centre can be overritten, and will continue to be updated from that point as usual.

### Scenes
A scene synchronises all actors and their actions to start and finish at the same time.
The constructor function takes in as parameters:
* A set of actors
* Number of iterations between start and finish
