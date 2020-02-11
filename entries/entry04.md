# Entry 4
##### 02/10/2020

# Summary
I set up the three.js project with basic 3D primitives and added some lights.

# Engineering Design Process
When I first started setting up the folder structure for the project, I tried to make everything as organized and structured as possible by creating classes. However I realized that by doing this, it only slowed down my progress because I was just writing code that I don't exactly know if I was going to need anyway. So instead, my approach is to write the code in one file and then from there, I will refactor my code as I need so that I can get straight into the project instead of writing code I might not even use in the future.

## Basic 3D primitives
Since I started writing my code before my partner made the game models, I used 3D primitives like cubes to act as placeholder so that I can start writing the logic to the game sooner.

### Lighting
When I placed my cubes initially, it looks like this, where everything just seems so jagged and you can't really tell the faces of the primitives apart from the rest.

```typescript
const game = new Game(); // Game has our scene, camera and renderer with antialiasing enabled

const wireframe = false;

const floor = new Mesh(
  new BoxGeometry(10, 10, 10, 10),
  new MeshBasicMaterial({ color: 0xde2e43, wireframe })
);

game.scene.add(floor);

const cube = new Mesh(
  new BoxGeometry(),
  new MeshBasicMaterial({ color: 0x5089db, wireframe })
);

cube.position.set(0, 7, 0);

game.scene.add(cube);
```

![](https://github.com/alexy4744/apcsa-freedom-project/blob/master/entry04-1.png?raw=true)

To make it look prettier, I added ambient and point lighting to the scene. Ambient lighting will make the whole scene look brighter while the point lighting will show the different sides of the primitives better. Lighting requires me to change the material of the mesh for the floor and cube to a `MeshPhongMaterial` since `MeshBasicMaterial` does not calculate lighting. So with lights, the code and scene becomes like this:

```typescript
const game = new Game();

const wireframe = false;

const floor = new Mesh(
  new BoxGeometry(10, 10, 10, 10),
  new MeshPhongMaterial({ color: 0xde2e43, wireframe })
);

game.scene.add(floor);

const cube = new Mesh(
  new BoxGeometry(),
  new MeshPhongMaterial({ color: 0x5089db, wireframe })
);

cube.position.set(0, 7, 0);

game.scene.add(cube);

const ambient = new AmbientLight(0xffffff, 0.75);

game.scene.add(ambient);

const light = new PointLight(0xffffff, 1.5, 18);

light.position.set(-3, 15, -3);

game.scene.add(light);
```

![](https://github.com/alexy4744/apcsa-freedom-project/blob/master/entry04-2.png?raw=true)

Already, the scene looks a lot better.

### Shadows
Now that we've got lighting, we can add shadows to the scene. First, we have to tell our WebGLRenderer compute shadows by enabling shadow mapping. Then, we have to make our floor receive a shadow from the cube and light that is casting it.

```typescript
game.renderer.shadowMap.enabled = true; // Enables shadow mapping on the renderer
game.renderer.shadowMap.type = PCFSoftShadowMap; // Makes shadows less jagged using PCSS

cube.castShadow = true;
light.castShadow = true;
cube.receiveShadow = true; // Not really necessary, only if you want the cube to also receive a shadow
floor.receiveShadow = true;

// Default values are 512 of the shadow mapping size, but increasing them makes the shadows even smoother
light.shadow.mapSize.width = 1024;
light.shadow.mapSize.height = 1024;
```

With shadows, the scene now looks like this:
![](https://github.com/alexy4744/apcsa-freedom-project/blob/master/entry04-3.gif?raw=true)

# Skills
I learned how to use Google to look up relevant information on shadow mapping such as different techniques. I also learned how that sometimes it is easier to not think ahead and work on what you need to do first before we improve on it, i.e. refactoring the code.

# Next Steps
Now that I have my three.js project set up, the next thing would be to import and load our GLTF models into the scene. After that we will need to add game logic and physics such as gravity so that players can actually fall of the arena aka our floor mesh. After that, we will have to make it multiplayer and implement a UI.

[Previous](entry03.md) | [Next](entry05.md)

[Home](../README.md)
