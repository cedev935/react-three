<h1>react-three-fiber</h1>

[![Version](https://img.shields.io/npm/v/react-three-fiber?style=flat&colorA=000000&colorB=000000)](https://npmjs.com/package/react-three-fiber)
[![Downloads](https://img.shields.io/npm/dt/react-three-fiber.svg?style=flat&colorA=000000&colorB=000000)](https://npmjs.com/package/react-three-fiber)
[![Twitter](https://img.shields.io/twitter/follow/pmndrs?label=%40pmndrs&style=flat&colorA=000000&colorB=000000&logo=twitter&logoColor=000000)](https://twitter.com/pmndrs)
[![Discord](https://img.shields.io/discord/740090768164651008?style=flat&colorA=000000&colorB=000000&label=discord&logo=discord&logoColor=000000)](https://discord.gg/ZZjjNvJ)
[![Open Collective](https://img.shields.io/opencollective/all/react-three-fiber?style=flat&colorA=000000&colorB=000000)](https://opencollective.com/react-three-fiber)
[![ETH](https://img.shields.io/badge/ETH-f5f5f5?style=flat&colorA=000000&colorB=000000)](https://blockchain.com/eth/address/0x6E3f79Ea1d0dcedeb33D3fC6c34d2B1f156F2682)
[![BTC](https://img.shields.io/badge/BTC-f5f5f5?style=flat&colorA=000000&colorB=000000)](https://blockchain.com/btc/address/36fuguTPxGCNnYZSRdgdh6Ea94brCAjMbH)

react-three-fiber is a <a href="https://reactjs.org/docs/codebase-overview.html#renderers">React renderer</a> for threejs.

```bash
npm install three @react-three/fiber
```

### Why?

Build dynamic scene graphs declaratively with re-usable, self-contained components that react to state changes, are interactive out of the box and can tap into React's ecosystem.

#### Does it have limitations?

None. Everything that works in threejs will work here.

#### Can it keep up with frequent updates?

It doesn't have to. It merely expresses threejs in JSX: `<mesh />` becomes `new THREE.Mesh()`. It doesn't know or target a specific threejs version nor does it need updates for modified, added or removed features.

#### Is it slower than plain threejs?

There is no additional overhead. Components participate in the renderloop outside of React, which takes care of managing the scene otherwise.

### What does it look like?

<table>
  <tr>
    <td>Let's make a re-usable component that has its own state, reacts to user-input and participates in the render-loop. (<a href="https://codesandbox.io/s/rrppl0y8l4?file=/src/App.js">live demo</a>).</td>
    <td>
      <a href="https://codesandbox.io/s/rrppl0y8l4">
        <img src="https://i.imgur.com/sS4ArrZ.gif" /></td>
      </a>
  </tr>
</table>

#### Imports first

```jsx
import { Canvas, useFrame } from '@react-three/fiber'
```

#### Define components

```jsx
function Box(props) {
  // This reference will give us direct access to the mesh
  const mesh = useRef()
  // Set up state for the hovered and active state
  const [hovered, setHover] = useState(false)
  const [active, setActive] = useState(false)
  // Rotate mesh every frame, this is outside of React without overhead
  useFrame(() => (mesh.current.rotation.x += 0.01))

  return (
    <mesh
      {...props}
      ref={mesh}
      scale={active ? 1.5 : 1}
      onClick={(event) => setActive(!active)}
      onPointerOver={(event) => setHover(true)}
      onPointerOut={(event) => setHover(false)}>
      <boxGeometry args={[1, 2, 3]} />
      <meshStandardMaterial color={hovered ? 'hotpink' : 'orange'} />
    </mesh>
  )
}
```

#### Compose your scene

```jsx
ReactDOM.render(
  <Canvas>
    <ambientLight />
    <pointLight position={[10, 10, 10]} />
    <Box position={[-1.2, 0, 0]} />
    <Box position={[1.2, 0, 0]} />
  </Canvas>,
  document.getElementById('root'),
)
```

<details>
  <summary>Show TypeScript example</summary>

```tsx
import { Canvas, MeshProps, useFrame } from '@react-three/fiber'

const Box: React.FC<MeshProps> = (props) => {
  // This reference will give us direct access to the mesh
  const mesh = useRef<THREE.Mesh>(null!)
  // Set up state for the hovered and active state
  const [hovered, setHover] = useState(false)
  const [active, setActive] = useState(false)
  // Rotate mesh every frame, this is outside of React without overhead
  useFrame(() => (mesh.current.rotation.x += 0.01))

  return (
    <mesh
      {...props}
      ref={mesh}
      scale={active ? 1.5 : 1}
      onClick={(event) => setActive(!active)}
      onPointerOver={(event) => setHover(true)}
      onPointerOut={(event) => setHover(false)}>
      <boxGeometry args={[1, 2, 3]} />
      <meshStandardMaterial color={hovered ? 'hotpink' : 'orange'} />
    </mesh>
  )
}

ReactDOM.render(
  <Canvas>
    <ambientLight />
    <pointLight position={[10, 10, 10]} />
    <Box position={[-1.2, 0, 0]} />
    <Box position={[1.2, 0, 0]} />
  </Canvas>,
  document.getElementById('root'),
)
```

</details>

---

# API

- [api.md](/markdown/api.md)
- [pitfalls.md](/markdown/pitfalls.md)
- [recipes.md](/markdown/recipes.md)
- [testing.md](/packages/test-renderer)

# Fundamentals

You need to be versed in both React and Threejs before rushing into this. If you are unsure about React consult the official [React docs](https://reactjs.org/docs/getting-started.html), especially [the section about hooks](https://reactjs.org/docs/hooks-reference.html). As for Threejs, make sure you at least glance over the following links:

1. Make sure you have a [basic grasp of Threejs](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene). Keep that site open.
2. When you know what a scene is, a camera, mesh, geometry, material, fork the [demo above](https://github.com/react-spring/react-three-fiber#what-does-it-look-like).
3. [Look up](https://threejs.org/docs/index.html#api/en/objects/Mesh) the JSX elements that you see (mesh, ambientLight, etc), _all_ threejs exports are native to three-fiber.
4. Try changing some values, scroll though our [Api](/markdown/api.md) to see what the various settings and hooks do.

Some reading material:

- [Threejs-docs](https://threejs.org/docs)
- [Threejs-examples](https://threejs.org/examples)
- [Threejs-fundamentals](https://threejsfundamentals.org)
- [Discover Threejs](https://discoverthreejs.com)
- [Do's and don'ts](https://discoverthreejs.com/tips-and-tricks) for performance and best practices
- [react-three-fiber alligator.io tutorial](https://alligator.io/react/react-with-threejs) by [@dghez\_](https://twitter.com/dghez_)

# Ecosystem

- [`@react-three/gltfjsx`](https://github.com/react-spring/gltfjsx) &ndash; turns GLTFs into JSX components
- [`@react-three/drei`](https://github.com/react-spring/drei) &ndash; useful helpers for react-three-fiber
- [`@react-three/postprocessing`](https://github.com/react-spring/react-postprocessing) &ndash; post-processing effects
- [`@react-three/flex`](https://github.com/react-spring/react-three-flex) &ndash; flexbox for react-three-fiber
- [`@react-three/xr`](https://github.com/react-spring/react-xr) &ndash; VR/AR controllers and events
- [`@react-three/cannon`](https://github.com/react-spring/use-cannon) &ndash; physics based hooks
- [`zustand`](https://github.com/react-spring/zustand) &ndash; state management
- [`react-spring`](https://github.com/react-spring/react-spring) &ndash; a spring-physics-based animation library
- [`react-use-gesture`](https://github.com/react-spring/react-use-gesture) &ndash; mouse/touch gestures

# How to contribute

If you like this project, please consider helping out. All contributions are welcome as well as donations to [Opencollective](https://opencollective.com/react-three-fiber), or in crypto `BTC: 36fuguTPxGCNnYZSRdgdh6Ea94brCAjMbH`, `ETH: 0x6E3f79Ea1d0dcedeb33D3fC6c34d2B1f156F2682`.

#### Backers

Thank you to all our backers! 🙏

<a href="https://opencollective.com/react-three-fiber#backers" target="_blank">
  <img src="https://opencollective.com/react-three-fiber/backers.svg?width=890"/>
</a>

#### Contributors

This project exists thanks to all the people who contribute.

<a href="https://github.com/pmndrs/react-three-fiber/graphs/contributors">
  <img src="https://opencollective.com/react-three-fiber/contributors.svg?width=890" />
</a>
