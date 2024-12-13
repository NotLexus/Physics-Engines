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
// module aliases
var Engine = Matter.Engine,
	Render = Matter.Render,
	Runner = Matter.Runner,
	Composites = Matter.Composites,
	Events = Matter.Events,
	Constraint = Matter.Constraint,
	MouseConstraint = Matter.MouseConstraint,
	Mouse = Matter.Mouse,
	Body = Matter.Body,
	Composite = Matter.Composite,
	Bodies = Matter.Bodies;

let pongA; //in progress

function setup() {
	// create an engine
	var engine = Engine.create();

	// create a renderer
	var render = Render.create({
		element: document.body,
		engine: engine,
	 options: {
	 	width: document.body.clientWidth,
		 	height: document.body.clientHeight,
			wireframes: false,
			background: ("orange")
		 }
	});

	// create objects and a ground
	pongA = Bodies.circle(400, 200, 20, {
		friction: 2,
		restitution: 0.7 })
	var pongB = Bodies.circle(500, 400, 40);
	var boxA = Bodies.rectangle(250, 50, 80, 80);
	var win = Bodies.rectangle(86, 950, 10, 10, {
		isStatic: true
	});
	var cpart2 = Bodies.rectangle(955, 975, 20, 150, {
		isStatic: true
	});
	var cpart3 = Bodies.rectangle(890, 975, 20, 100, {
		isStatic: true
	});
	var table = Bodies.rectangle(890, 1015, 400, 100, {
		isStatic: true
	});
	var ground = Bodies.rectangle(800, windowHeight, 100000, 60, {
		isStatic: true
	});

	// add all of the bodies to the world
	Composite.add(engine.world, [pongA, pongB, boxA, win, cpart2, cpart3, table]);

	rockOptions = {
			density: 0.004
		},
		rock = Bodies.polygon(170, 450, 8, 20, rockOptions),
		anchor = {
			x: 170,
			y: 750
		},
		elastic = Constraint.create({
			pointA: anchor,
			bodyB: rock,
			length: 0.01,
			damping: 0.01,
			stiffness: 0.01
		});

	Composite.add(engine.world, [rock, elastic]);

    Events.on(engine, 'afterUpdate', function() {
        if (mouseConstraint.mouse.button === -1 && (rock.position.x > 190 || rock.position.y < 430)) {
            // Limit maximum speed of current rock.
            // if (Body.getSpeed(rock) > 45) {
            //     Body.setSpeed(rock, 45);
            // }

            // Release current rock and add a new one.
            rock = Bodies.polygon(170, 450, 7, 20, rockOptions);
            Composite.add(engine.world, rock);
            elastic.bodyB = rock;
        }
    });

	// add mouse control
	var mouse = Mouse.create(render.canvas),
		mouseConstraint = MouseConstraint.create(engine, {
			mouse: mouse,
			constraint: {
				stiffness: 0.2,
				render: {
					visible: false
				}
			}
		});

	Composite.add(engine.world, mouseConstraint);

	// run the renderer
	Render.run(render);

	// create runner
	var runner = Runner.create();

	// run the engine
	Runner.run(runner, engine);

}
