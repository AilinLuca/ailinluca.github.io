---
layout: post
title: React Best Practices Collection 2022
categories: react 
permalink: /:title
---

### 10 React Antipatterns to Avoid

1. Break up big components. 
- Use glean VSCode extension to quickly break up components.

2. Do not nest the child component declaration.
- You will see unusual behavior. 

Wrong:

```
function Parent() {
  const [count, setCount] = useState(0);
  const handleClick = () => setCount(count + 1);
  const Child = () => {
    return <button onClick={handleClick}>+</button>
  }

  return (
    <div>
      <Child />
    </div>
  )
}
```

Right: 
```
const Child = ({onClick}) => {
    return <button onClick={onClick}>+</button>
}

function Parent() {
  const [count, setCount] = useState(0);
  const handleClick = () => setCount(count + 1);

  return (
    <div>
      <Child onClick={handleClick} />
    </div>
  )
}
```

3. useMemo() to only refresh value when dependent changes and useCallback only when function changes

4. Use fragments <></> to keep the tree light.

5. Organize files: One component per file, separate folders per component for larger applications.
- As app becomes more complex, each component has its own folder.
- e.g. Navbar folder contains Navbar.tsx, Navbar.module.css, Navbar.spec.ts, Navbar.stories.js, and index.tsx barrel for easy export

6. Import dynamically (lazy load) to speed up page load times for larger components. 

```
import React, { Suspense } from 'React';
const Button = React.lazy(() => import('./Button'));

function Page() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Button />
    </Suspense>
  );
}
```

7. Avoid prop drilling, 
- BAD: Passing props through many layers of children to reach a specific nested child. 
- GOOD: Use context

```
function PropDrilling() {
  const [count] = useState(0);
  return (
    <CountContext.Provider value={count}>
      <Child />
    </CountContext.Provider>
  )
}

function Child() {
  return <GrandChild />
}

function GrandChild() {
  const count = useContext(CountContext);
  return <div>{count}</div>
}
```
- However use context sparingly because it makes components dependent on context. Very few values are truly global. 
- Question: Are there component-specific contexts? 

8. Prop plowing
- Spread props to avoid passing a long list of props
- BAD:
```
<User id={data.id} name={data.name} bio={data.bio} age={data.age} avatar={data.avatar}>
```
- GOOD: 
```
<User {...data}>
```
- However it does make code less explicit, so only use when you have a LOT

9. Use curry functions
- BAD: Too many event handlers looks messy
```
const handleIt = (e: any, v: number) => {
  console.log(e,v);
}

return <>
  <div>
    <input onChange={(e) => handleIt(e, 1)}>
    <input onChange={(e) => handleIt(e, 2)}>
    <input onChange={(e) => handleIt(e, 3)}>
  </div>
</>
```
- GOOD: Use curry function
```
const handleIt = (v: number) => {
  return (e: any) => console.log(e,v);
}

return <>
  <div>
    <input onChange={handleIt(1)}>
    <input onChange={handleIt(2)}>
  </div>
</>
```

10. Try not to put all your data in one object.
- No longer more performant to have fewer setState calls. React 18 does automatic batching whenever you update the state, so that even if you call setState multiple times in a function will not trigger multiple rerenders.
- This makes it harder to extract as a custom hook.
- Back in the day, it was common to create smart components that control the data and dumb components that just render it with props.
- Now it's easier to extract logic into a custom hook. 

```
function Feature() {
  const [state, setState] = useState({});

  const [count, setCount] = useState(0);
  const [coords, setCoords] = useState({x:0, y:0});

  const { title, theme, ts } = useMetadata();
}

// custom hook handles the more complex business logic
function useMetadata() {
  const [title, setTitle] = useState('hey');
  const [theme, setTheme] = useState('dark');
  const [ts, setTs] = useState(Date.now());

  return { title, theme, ts };
}

```

Source: [https://www.youtube.com/watch?v=b0IZo2Aho9Y](https://www.youtube.com/watch?v=b0IZo2Aho9Y)