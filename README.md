# Moovee
Moovee enables you to form complex and structured animations using relatively high-level descriptions. It works on an intricate events-based system to give control of every component of the animation, should you choose to make specific configurations.

## Terminology
Moovee uses film-making terminology to provide highly intuitive and readable scripting. The library that allows for versatile, complex rates-of-change (`Actions`), which can be attached to renderable objects (`Actors`) and then grouped into synchornised animations (`Scenes`), all aggregated together with user configurations to be played back as a `Movie`.

## Example
The following code glides a box smoothly across the screen (symmetrical quadratic derivative) while spinning at an increasing rate (linear derivative):
```c++
// define fundamental animation components
Vec2 pFrom   = {100, 100},  // starting position
     pTo     = {300, 500},  // ending position
     pMinMax = {0, 10};     // min and max speeds

Vec2 aFrom   = {0, 0},      // starting angle
     aTo     = {PI / 2, 0}, // ending angle
     aMinMax = {0, 0.1};    // min and max angular velocities

// initialise scripts
Script* pScript = new Script(Actions::symQuad, {pFrom, pTo, pMinMax}, MV_TRANSLATION); // script for position
Script* aScript = new Script(Actions::linear, {aFrom, aTo, aMinMax}, MV_ROTATION);     // script for angle

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
     actors[0].renderAsGeometry();
     return true;
};

// play scene
scene.play();

``` 
