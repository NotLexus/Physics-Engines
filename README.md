# Physics-Engines
physics

// module aliases
var Engine = Matter.Engine,
    Render = Matter.Render,
    Runner = Matter.Runner,
    Bodies = Matter.Bodies,
    Composite = Matter.Composite;

// create an engine
var engine = Engine.create();

// create a renderer
var render = Render.create({
    element: document.body,
    engine: engine
});

// create objects and a ground
var pongA = Bodies.circle(400, 200, 20);
var pongB = Bodies.circle(400, 400, 40);
var boxA = Bodies.rectangle(450, 50, 80, 80);
var table = Bodies.rectangle(400, 550, 400, 60, { isStatic: true }); 
var ground = Bodies.rectangle(400, 610, 810, 60, { isStatic: true });

// add all of the bodies to the world
Composite.add(engine.world, [pongA, pongB ,boxA, table, ground]);



// run the renderer
Render.run(render);

// create runner
var runner = Runner.create();

// run the engine
Runner.run(runner, engine);
