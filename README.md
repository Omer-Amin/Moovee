# Moovee
Moovee is an incredibly versatile 2D animation library. It utilises a modular design to allow you to monitor and fine tune each and every aspect of the animation in real time, allowing for features such as interactibility.

The rendering component of Moovee is an optional module, as the library contains a vast number of useful methods for linear algebra, geometry, incrementation, interpolation, calculus, and more.

The script is highly intuitive. The following example morphs a circle into a square:

```c++
Render::create(600, 600);

shape circle = Shape::circ(300, 300, 100),
      square = Shape::rect(300, 300, 100, 100);

Movie myMovie = Movie({
    Scene(Script::morph(circle, rectangle))
});

myMovie.afterFrame = [&](Movie& movie) -> void {
    Render::clear();
    Render::setColor(RED);
    Sketch::scene(movie.showing);
    Render::render();
};

Movie::play(myMovie);
```

