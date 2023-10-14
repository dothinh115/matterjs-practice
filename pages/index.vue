<template>
  <div class="container mx-auto md:h-[753px] h-screen md:mt-[100px] relative">
    <div
      class="absolute w-full h-fit flex justify-center items-center text-center top-[100px] md:text-[30px] text-[20px] text-slate-500 font-bold"
    >
      Thịnh Đỗ with love <br />
      ❤️❤️❤️
    </div>
    <div
      id="canvas"
      class="h-full w-full md:border-gray-200 md:border md:rounded-[12px]"
    ></div>
  </div>
</template>
<script setup lang="ts">
import Matter from "matter-js";
let width: number, height: number, el: HTMLDivElement | null;
onMounted(() => {
  el = document.querySelector("#canvas");
  width = el?.getBoundingClientRect().width || 0;
  height = el?.scrollHeight || 0;
  createMatterJs();
});
const createMatterJs = () => {
  if (!el) return;
  // module aliases
  const { Engine, Render, Runner, Bodies, Composite, Mouse, MouseConstraint } =
    Matter;

  // create an engine
  const engine = Engine.create();

  // create a renderer
  const render = Render.create({
    element: el,
    engine: engine,
    options: {
      wireframes: false,
      width,
      height,
      background: "transparent",
      wireframeBackground: "transparent",
    },
  });
  const mouse = Mouse.create(render.canvas),
    mouseConstraint = MouseConstraint.create(engine, {
      mouse: mouse,
      constraint: {
        stiffness: 0.2,
        render: {
          visible: false,
        },
      },
    });

  // add all of the bodies to the world
  const items = createItems(Bodies);
  const viewPort = createViewPort(Bodies);
  // run the renderer
  Render.run(render);

  // create runner
  var runner = Runner.create();

  // run the engine
  Runner.run(runner, engine);
  Composite.add(engine.world, [...items, ...viewPort, mouseConstraint]);
};

const createViewPort = (Bodies: any): any[] => {
  const offset = 20;
  const options = {
    isStatic: true,
    render: {
      lineWidth: 0,
      fillStyle: "transparent",
    },
  };
  const bottom = Bodies.rectangle(width / 2, height, width, offset, options);
  const left = Bodies.rectangle(0, height / 2, offset, height, options);
  const right = Bodies.rectangle(width, height / 2, offset, height, options);
  const top = Bodies.rectangle(width / 2, 0, width, offset, options);
  return [bottom, left, right, top];
};

const createItems = (Bodies: any): any[] => {
  let arr: any[] = [];
  let number: number = 40;
  let size: number = 45;
  if (window.innerWidth < 768) {
    size = 30;
  }
  for (let i = 0; i < number; i++) {
    const x = Math.random() * width;
    arr = [
      ...arr,
      Bodies.circle(x, 0, size, {
        density: 0.0005,
        frictionAir: 0.06,
      }),
    ];
  }
  return arr;
};
</script>
