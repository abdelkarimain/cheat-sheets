# React Cheat Sheet

React est une librairie vraiment extraordinaire. Par contre, elle n'est pas si simple et il y a beaucoup de chose qu'il faut mémoriser. C'est pourquoi j'ai fait ce monstrueux aide-mémoire avec tous les concepts de base de React.

## Création d'une application React

```
npx create-react-app my-app-name

# Exécuter le serveur local
cd my-app-name
yarn start

# http://localhost:3000
```

## Règles pour la création d'un component React

- La fonction doit avoir la première lettre de son nom en majuscule
- La fonction doit retourner du JSX

Exemple :

```javascript
// src/App.js
// React component
function App() {
  return <h1>Hello World</h1>;
}

export default App;
```

Comment ce composant est-il rendu dans le navigateur ?

Le fichier principal du projet est `src/index.js` et dans ce fichier il y a des instructions pour le rendu du component

```javascript
ReactDOM.render(<App />, document.getElementById('root'));
```

Le component `App` sera ensuite rendu dans `public/index.html` au niveau de la `div` avec l'ID `root`.

## Importer un component

Les components React seront créés dans des fichiers séparés. Chaque component doit être exporté puis importé.

```javascript
// src/Greeting.js
function Greeting() {
  return <h1>Hello World</h1>;
}

export default Greeting;
```

```javascript
// src/App.js
import Greeting from './Greeting';

function App() {
  return <Greeting />;
}
```

## Règles d'utilisation du JSX

- Renvoie un seul élément (un seul élément parent)

```javascript
// Non valide
return <h1>Hello world</h1><h2>Hi!</h2>;

// Valide grâce à l'utilisation du tag <fragment>
return (
  <>
    <h1>Hello World</h1>
    <h2>Hi!</h2>
  </>
);
```

- Utiliser `className` au lieu de `class`
- Tous les noms d'attributs doivent être camelCase

```javascript
// Non valide
return (
  <div class="title">
    Hello World
  </div>
);

// Valide
return (
  <div className="title">
    Hello World
  </div>
);
```

- Fermez le tag de chaque élément

```javascript
return (
  <img src="http://example.com/image.jpg" />
  <input type="text" name="first_name" />
);
```

## Nested Components

```javascript
// Arrow function shorthand component
const Person = () => <h1>Mike Taylor</h1>;

// Arrow function component
const Message = () => {
  return <h1>Hello</h1>;
};

// Function component
function HelloWorld() {
  return (
    <>
      <Message />
      <Person />
    </>
  );
}
```

## Component CSS

**App.css**

```css
h1 {
  color: red;
}
```

**App.js**

```javascript
import './App.css';

function App() {
  return <h1>Hello World</h1>;
}
```

## Inline CSS

```javascript
function App() {
  return <h1 style={{ color: 'red' }}>Hello World</h1>;
}
```

## JavaScript in JSX

- Écrire entre `{}` 
- Doit être une expression (retourner une valeur)

```javascript
function App() {
  const name = 'Mike';
  return (
    <>
      <h1>Hello {name}</h1>
      <p>{name === 'Mike' ? '(admin)' : '(user)'}</p>
    </>
  );
}
```

## Component Properties (Props)

```javascript
function App() {
  return <Person name='Mike' age={29} />;
}

const Person = (props) => {
  return <h1>Name: {props.name}, Age: {props.age}</h1>;
}

// or props object deconstructing
const Person = ({name, age}) => {
  return <h1>Name: {name} Age: {age}</h1>;
}
```

## Children Props (slot)

```javascript
function App() {
  return (
    <Person name='Mike' age={29}>
      Hi, this is a welcome message
    </Person>
  );
}

const Person = (props) => {
  return (
    <>
      <h1>Name: {props.name}, Age: {props.age}</h1>
      <p>{props.children}</p>
    </>
  );
}

// or props object deconstructing
const Person = ({name, age, children}) => {
  return (
    <>
      <h1>Name: {name} Age: {age}</h1>
      <p>{children}</p>
    </>
  );
}
```

## Default Props value

```javascript
const Person = ({name, age, children}) => {
  return (
    <>
      <h1>Name: {name} Age: {age}</h1>
      <p>{children}</p>
    </>
  );
}

Person.defaultProps = {
  name: 'No name',
  age: 0,
};
```

## List

```javascript
const people = [
  {id: 1, name: 'Mike', age: 29},
  {id: 2, name: 'Peter', age: 24},
  {id: 3, name: 'John', age: 39},
];

function App() {
  return (
    people.map(person => {
      return <Person name={person.name} age={person.age}/>;
    })
  );
}

const Person = (props) => {
  return <h1>Name: {props.name}, Age: {props.age}</h1>;
}
```

## List with key (for React internal reference)

```javascript
function App() {
  return (
    people.map(person => {
      return <Person key={person.id} name={person.name} age={person.age}/>;
    })
  );
}
```

## Props object destructuring

```javascript
function App() {
  return people.map(person => <Person key={person.id} {...person} />);
}

const Person = ({name, age}) => {
  return <h1>Name: {name}, Age: {age}</h1>;
}
```

## Click Event

```javascript
const clickHandler = () => alert('Hello World');

function App() {
  return (
    <>
      <h1>Welcome to my app</h1>
      <button onClick={clickHandler}>Say Hi</button>
    </>
  );
}
```

ou inline...

```javascript
function App() {
  return (
    <>
      <h1>Welcome to my app</h1>
      <button onClick={() => alert('Hello World')}>Say Hi</button>
    </>
  );
}
```

Pour passer des arguments, nous devons utiliser les arrow function

```javascript
const clickHandler = (message) => alert(message);

function App() {
  return (
    <>
      <h1>Welcome to my app</h1>
      <button onClick={() => clickHandler('Hello World')}>Say Hi</button>
    </>
  );
}
```

## e pour event arguments

```javascript
const clickHandler = (e) => console.log(e.target);

function App() {
  return (
    <>
      <h1>Welcome to my app</h1>
      <button onClick={clickHandler}>Say Hi</button>
    </>
  );
}
```

## Passer event du child au parent

```javascript
function Todo({item, onDelete}) {
  return (
    <div>
      {item}
      <button onClick={() => onDelete(item)}>Delete</button>
    </div>
  );
}

function Todos() {
  const handleDelete = (todo) => {
    const newTodos = todos.filter(item => item !== todo);
    setTodos(newTodos);
  };

  return (
    <>
      {todos.map(todo => (
        <Todo key={todo} item={todo} onDelete={handleDelete}/>
      ))}
    </>
  );
}
```

## useState Hook

Le but de `useState` est de gérer les données réactives. Toute donnée qui change dans l'application est appelée 'state'. Et lorsque l'état change, vous souhaitez réagir pour mettre à jour l'interface utilisateur.

- Les Hook commencent toujours par le préfixe 'use'
- Doit être invoqué uniquement dans un component React
- Doit être appelé au plus haut niveau d'un component
- La déclaration ne peut pas être appelée conditionnellement
- `useState` renvoie un tableau : [valeur d'état, fonction d'état définie]

```javascript
import React, { useState } from 'react';

const DisplayTitle = () => {
  const [title, setTitle] = useState('This is the Title');
  const handleClick = () => setTitle('New Title');

  return (
    <>
      <h2>{title}</h2>
      <button type="button" className="btn" onClick={handleClick}>
        Change Title
      </button>
    </>
  );
};

export default DisplayTitle;
```

## Multiple State values

```javascript
const Todo = ({item, onDelete}) => {
  return (
    <div>
      {item}
      <button onClick={() => onDelete(item)}>Delete</button>
    </div>
  );
}

const Todos = () => {
  const [todo, setTodo] = useState('');
  const [todos, setTodos] = useState([]);

  const handleSubmit = (e) => {
    e.preventDefault();
    setTodos([...todos, todo]);
    setTodo('');
  }

  const handleDelete = (todo) => {
    setTodos(todos.filter(item => item !== todo));
  };

  return (
    <>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={todo}
          onChange={(e) => setTodo(e.target.value)}
        />
        <button type="submit">Add Todo</button>
      </form>
      {todos.map(todo => (
        <Todo key={todo} item={todo} onDelete={handleDelete}/>
      ))}
    </>
  );
}
```

## useState Functional Updates

```javascript
const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  };

  return (
    <>
      <h1>Counter: {count}</h1>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </>
  );
}
```

## useEffect Hook

Le but de `useEffect` est de gérer les effets secondaires dans un component, par exemple, charger les données depuis une API, écouter des événements, etc.

- Toujours utiliser 'use' au début du nom
- Doit être invoqué uniquement dans un component React
- Doit être appelé au plus haut niveau d'un component
- Peut être utilisé plusieurs fois dans un component
- Peut dépendre des valeurs de state et de props

```javascript
import { useEffect, useState } from 'react';

const FetchData = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(data => setData(data));
  }, []); // la dépendance vide signifie que ce useEffect ne sera exécuté qu'une seule fois après le premier rendu

  return (
    <ul>
      {data.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
};
```

## Conditional Rendering

```javascript
const App = () => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <>
      {isLoggedIn ? (
        <h1>Welcome back!</h1>
      ) : (
        <h1>Please log in.</h1>
      )}
      <button onClick={() => setIsLoggedIn(!isLoggedIn)}>
        {isLoggedIn ? 'Log out' : 'Log in'}
      </button>
    </>
  );
};
```

## Controlled Inputs

```javascript
const App = () => {
  const [value, setValue] = useState('');

  const handleChange = (e) => setValue(e.target.value);

  return (
    <>
      <input type="text" value={value} onChange={handleChange} />
      <p>{value}</p>
    </>
  );
};
```

## Context API

La Context API est utilisée pour passer les données profondément dans l'arborescence des components sans avoir à passer les props manuellement à chaque niveau.

```javascript
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

const App = () => {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  );
};

const Toolbar = () => {
  return (
    <div>
      <ThemedButton />
    </div>
  );
};

const ThemedButton = () => {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <button
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
      style={{ background: theme === 'light' ? '#fff' : '#333', color: theme === 'light' ? '#000' : '#fff' }}
    >
      Change Theme
    </button>
  );
};
```

## Custom Hooks

Les Custom Hooks permettent de réutiliser la logique dans les components.

```javascript
import { useState, useEffect } from 'react';

const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(response => response.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
};

const App = () => {
  const { data, loading } = useFetch('https://jsonplaceholder.typicode.com/posts');

  if (loading) {
    return <p>Loading...</p>;
  }

  return (
    <ul>
      {data.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
};
```

## Router

Pour la navigation dans une application React, `react-router-dom` est couramment utilisé.

```bash
yarn add react-router-dom
```

**App.js**

```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch, Link } from 'react-router-dom';

const Home = () => <h1>Home Page</h1>;
const About = () => <h1>About Page</h1>;

const App = () => {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  );
};

export default App;
```

## Redux

Redux est un conteneur d'état prévisible pour les applications JavaScript.

```bash
yarn add redux react-redux
```

**store.js**

```javascript
import { createStore } from 'redux';

const initialState = { value: 0 };

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 };
    case 'DECREMENT':
      return { value: state.value - 1 };
    default:
      return state;
  }
};

const store = createStore(reducer);

export default store;
```

**App.js**

```javascript
import React from 'react';
import { Provider, useDispatch, useSelector } from 'react-redux';
import store from './store';

const Counter = () => {
  const dispatch = useDispatch();
  const value = useSelector(state => state.value);

  return (
    <>
      <h1>Counter: {value}</h1>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrement</button>
    </>
  );
};

const App = () => (
  <Provider store={store}>
    <Counter />
  </Provider>
);

export default App;
```

## Tests

Pour tester les components React, vous pouvez utiliser `jest` et `react-testing-library`.

```bash
yarn add jest @testing-library/react
```

**Counter.test.js**

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('renders counter', () => {
  render(<Counter />);
  const counterElement = screen.getByText(/Counter:/i);
  expect(counterElement).toBeInTheDocument();
});

test('increments counter', () => {
  render(<Counter />);
  const buttonElement = screen.getByText(/Increment/i);
  fireEvent.click(buttonElement);
  const counterElement = screen.getByText(/Counter: 1/i);
  expect(counterElement).toBeInTheDocument();
});
```

## Optimization

React.memo et `useCallback` sont utilisés pour l'optimisation des components.

**React.memo**

```javascript
const ExpensiveComponent = React.memo(({ data }) => {
  // render the component
});

const ParentComponent = () => {
  const [data, setData] = useState([]);
  return <ExpensiveComponent data={data} />;
};
```

**useCallback**

```javascript
const ParentComponent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <ChildComponent onClick={handleClick} />;
};

const ChildComponent = React.memo(({ onClick }) => {
  return <button onClick={onClick}>Click me</button>;
});
```
