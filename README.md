# Moovee
Moovee enables you to form complex and structured animations using relatively high-level descriptions. It works on an intricate events-based system to give control of every component of the animation, should you choose to make specific configurations.

## Terminology
Moovee uses film-making terminology to provide highly intuitive and readable scripting. The library that allows for versatile, complex rates-of-change (`Actions`), which can be attached to renderable objects (`Actors`) and then grouped into synchornised animations (`Scenes`), all aggregated together with user configurations to be played back as a `Movie`.

## Example
The following code defines a rectangular `Actor` which glides smoothly across the screen (symmetrical quadratic derivative) while spinning at an increasing rate (linear derivative):
```c++
// create set of vertices
std::vector<Vec2> vertices = {
  {10, 10},
  {20, 10},
  {20, 20},
  {10, 20}
};

// create angle vector (only utilising x component)
std::vector<float> a = {0, 0};

// define fundamental animation components
Vec2 vFrom   = {100, 100},  // starting position
     vTo     = {300, 500},  // ending position
     vMinMax = {0, 10};     // min and max speeds

Vec2 aFrom   = {0, 0},      // starting angle
     aTo     = {PI / 2, 0}, // ending angle
     aMinMax = {0, 0.1}     // min and max angular velocities

// initialise scripts
Script* vScript = new Script(v, Actions::symQuad, {vFrom, vTo, vMinMax}, MV_TRANSLATION); // script for vertices
Script* aScript = new Script({a}, Actions::linear, {aFrom, aTo, aMinMax}, MV_ROTATION);   // script for angle

// create an actor
Actor* actor = new Actor(vertices, {vScript, aScript});

// create scene for a synchronised animation that reaches the final state in 1000 iterations
Scene* scene = new Scene({actor}, 1000);

// after each iteration, render
scene.afterTick = [](std::vector<Actor*> actors) -> bool {
  actors[0].renderAsGeometry();
};

// play scene
scene.play();

```
