# header

```js
import React from "react";
```

---

# useState

```js
import React, { useState } from "react"; ///
const [title, setTitle] = useState(props.title);

// changing title
setTitle("hdj"); //title changes to 'hdj
```

- react rerenders form each time useState changes

---

# dealing with useState with multiple details

```js
user={
    name,date,amount
}
const [user, setUser] = useState(user);
//when we need to change one value
setUser(...user,name:'newname') //here we change only name of user
```

- the above code should not be done like this as React schedules the update not instantly and maybe the prevstate is wrong

```js
const titleChangeHandler = (e) => {
  setItem((prev) => {
    return { ...prev, title: e.target.value };
  });
};
//prev is the prev state of user before change is made
```

---

```js
const ExpenseItem = (props) => {
  const [title, setTitle] = useState(props.title);
  const clickHandler = () => {
    setTitle("Updated");
  };
  return (
    <Card className="expense-item">
      <ExpenseDate date={props.date} />
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">{props.amount}</div>
        <button onClick={clickHandler}>{title}</button>
      </div>
    </Card>
  );
};
```

- in this title is updated only in that component in which button was clicked no others

---

# passing function to child

`NewExpense.js`

```js
const outerfunc = (enteredvalu) => {
  console.log(enteredval);
};
return (
  <div className="new-expense">
    <ExpenseForm onsave={outerfunc} />
  </div>
);
```

- onsave becomes a prop
  in `ExpenseFor.js`

```js
const ExpenseForm = (props) => {
  //running it inside child
  props.onsave("fksd");
};
```

---

# container props

```js
<Card className="expenses">
  //////////////////////children/////////////
  //////////////////////////////////////////
</Card>
```

```js
import "./Card.css";
const Card = (props) => {
  const classes = "card " + props.className; //we do this so that card can use its own css and the css from classnme

  return <div className={classes}>{props.children}</div>;
};
```

- props.children are the items inside `Card`

---

# adding wrappers so that we know why that div is there

```js
<div>  //app wrapper
  <div> //expense wrapper
      <ErrorModal/>
      <User>
  </div>
</div>
```

- end result
- so we can make wrapper components instead of using div which does not tell us anything

```js
const ExpensseWrapper = (props) => {
  return props.children;
};
```

then above code

```js
<div>
  <ExpenseWrapper>
      <ErrorModal/>
      <User>
  </Expense
```

---

or use `React Fragments`

```js
<> //or //  <React.Fragment>
      <ErrorModal/>
      <User>
</>
```

# `React Portals`

```js
const ErrorModal = (props) => {
  return (
    <>
      <div className={classes.backdrop} onClick={props.onConfirm} />//////////////// backdrop ///////////////
      <Card className={classes.modal}> ////modal
        <header className={classes.header}>
          <h2>{props.title}</h2>
        </header>
        <div className={classes.content}>
          <p>{props.message}</p>
        </div>
        <footer className={classes.actions}>
          <Button onClick={props.onConfirm}>Okay</Button>////////
        </footer>
      </Card>
    </>
```

- in this error modal we have 2 things we can click to make it disappear
  - backdrop
  - button in the modal
- so backdrop must start form the begining of page
- if we place inside Error Modal
- we will begin backdrop from where ErrorModal is rendered in the page so to put it at the begining of the page from the start we use portals

---

`Error Modal.js`

```js
const Backdrop = (props) => {
  // Backdrop
  return <div className={classes.backdrop} onClick={props.onConfirm} />;
};

const ModalOverlay = (props) => {
  //modal
  return (
    <Card className={classes.modal}>
      <header className={classes.header}>
        <h2>{props.title}</h2>
      </header>
      <div className={classes.content}>
        <p>{props.message}</p>
      </div>
      <footer className={classes.actions}>
        <Button onClick={props.onConfirm}>Okay</Button>
      </footer>
    </Card>
  );
};

const ErrorModal = (props) => {
  return (
    <React.Fragment>
      {ReactDOM.createPortal(
        <Backdrop onConfirm={props.onConfirm} />,
        document.getElementById("backdrop-root") ////////////
      )}

      {ReactDOM.createPortal(
        <ModalOverlay
          title={props.title}
          message={props.message}
          onConfirm={props.onConfirm}
        />,
        document.getElementById("overlay-root") //adds this part to that id in html page
      )}
    </React.Fragment>
  );
};
```

`index.html` for above page

```html
<div id="backdrop-root"></div>
//where backdrop is sent
<div id="overlay-root"></div>
//location where modal is sent
<div id="root"></div>
//where the APP.js renders
```

---

# useRef

- points to the line which stores the ref

```js
<input
  id="username"
  type="text"
  // value={enteredUsername}
  // onChange={usernameChangeHandler}
  ref={enteredUsernameRef}
/>
```

`enteredUsername`= input Dom

- so by userref we get the dom and we can manipulate it now

```js
  const enteredAgeRef = useRef();
  const [error, setError] = useState();

  const addUserHandler = (event) => {

    event.preventDefault();
    const enteredUsername = enteredUsernameRef.current.value;
    const enteredAge = enteredAgeRef.current.value;
```

here after form submission we can get the values un those fields

---

# useEffect()

![](img\useEffect.png)

```js
useEffect(() => {
  setFormIsValid(
    enteredEmail.includes("@") && enteredPassword.trim().length > 6
  );
}, [enteredEmail, enteredPassword]);
```

- useEffect Renders the page every time enteredEmail or enteredPassword change

---

```js
useEffect(() => {}, []);
```

- useEffect with no dependencies run only once during the rendering of the page after which it doesnt run

---

# cleanup

```js
const UseEffectCleanup = () => {
  const [size, setSize] = useState(window.innerWidth);

  const checkSize = () => {
    setSize(window.innerWidth);
  };

  useEffect(() => {
    console.log("useEffect");
    window.addEventListener("resize", checkSize);
    return () => {
      console.log("cleanup");
      window.removeEventListener("resize", checkSize);
    };//everytime the page renders when window size increases a new eventlistner is added so we need to delete one before adding another
  }, []);
```

- so we add a return statement so that evry time page re renders the prev useEffect return statement runs so before adding the second even lister we remove the previous one

- so if you reload the page and page rerenders then one event listener is removed

---

```js
useEffect(() => {
  //effect
  return () => {
    //cleanup
  };
}, [dependency]);
```

- everytime a dependency changes the previous effect is removed with cleanup and then new effect takes place

---

# useReducer

![](img\useReducer.png)

---

`Example:`

```
to build a state to check if email is valid:
- email is valid if it contains '@' and has
- more than 6 characters
and we need to check if form is valid if
- check if valid if on blurrr
```

```js
const emailReducer = (state, action) => {
  if (action.type === 'USER_INPUT') {
    return { value: action.val, isValid: action.val.includes('@') };
  }
  if (action.type === 'INPUT_BLUR') {
    return { value: state.value, isValid: state.value.includes('@') };
  }
  return { value: '', isValid: false };
};
// the above should be above component
const Login=(prop)=>{

  const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: '',
    isValid: null,
  });

  // the below runs when user is changing email it automaticaly checks if email is valid and if form is valid
  const emailChangeHandler = (event) => {
    //check if email is valid
    dispatchEmail({type: 'USER_INPUT', val: event.target.value});
    );
  };
  const validateEmailHandler = () => { //here we are not inputin gvalue as it doesnt change
    dispatchEmail({ type: 'INPUT_BLUR' });
  };
//
/////////////////////////////////////////////
          <input
            type="email"
            id="email"
            value={emailState.value}
            onChange={emailChangeHandler}
            onBlur={validateEmailHandler}
          />
//////////email/////////////////////////////
```

- so in the above if email is changed we go to User_INPUT
- or if the focus is not in form we go to INPUT_BLUR
- the same can be done for password too

---

# to us this to check if form is valid

```js
useEffect(() => {
  setTimeout(() => {
    setformIsValid(emailState.isValid && passwordState.isValid);
  }, 500);
  return () => {
    clearTimeout();
  };
}, [emailState, passwordState]);
```

- if `emailState.value` or `passwordState.value` is changed we go to check if both are valid after 5 seconds
- if they change again before 5 s we go to cleanup which removes that timer and starts a timer for new value

---

- we need to only rerender when isvalid changes so

## `object destructuring`

```js
// Grabs obj.x as as { otherName }
const { x: otherName } = obj;
```

use this technique

```js
const { isValid: emailIsValid } = emailState;
```

so the code becomes

```js
const { isValid: emailIsValid } = emailState;
const { isValid: passwordIsValid } = passwordState;
useEffect(() => {
  setTimeout(() => {
    setformIsValid(emailState.isValid && passwordState.isValid);
  }, 500);
  return () => {
    clearTimeout();
  };
}, [emailIsValid, passwordIsValid]);
```

---

# UseContext

![](img\problemswithliftingupdata.png)
problem
![](img\useContexteg.png)

auth context.js

```js
import React from "react";

const AuthContext = React.createContext({
  isLoggedIn: false,
  onLogout: () => {}, //empty as it will be provided later
});

export default AuthContext;
```

App.js

```js
//importing Authcontext
import AuthContext from "./store/auth-context";
/////the component returned
return (
  <AuthContext.Provider
    value={{
      isLoggedIn: isLoggedIn, //  => providing values
      onLogout: logoutHandler, //passing function
    }}
  >
    <MainHeader />
    <main>
      {!isLoggedIn && <Login onLogin={loginHandler} />}
      {isLoggedIn && <Home onLogout={logoutHandler} />}
    </main>
  </AuthContext.Provider>
);
```

Navigation.js

```js
import AuthContext from '../../store/auth-context';
import classes from './Navigation.module.css';

const Navigation = () => {
  const ctx = useContext(AuthContext); //using useContext to get vcontextobject which has all values
  //////////////////////
  return(
  {ctx.isLoggedIn && (
  /////at some point of code
  <button onClick={ctx.onLogout}>Logout</button>
  /////
  );
}
```

- so as seen Nabigation gets its values without passing it through function by accessin Context
- useContext should only be used for ery log prop chains and not small ones
- is not built for high frequency changes

---

# useImperative and forwareded Refs

- if you want to do some action on a child component form parent component we need ref of it but we cannot send refs to parent normally
  `Input.js` a component used in `login.js` as a modified input field

```js
const Input = React.forwardRef((props, ref) => { ///
  const inputRef = useRef();

  const activate = () => {
    inputRef.current.focus();
  };

  useImperativeHandle(ref, () => { //here it sends the activate function to the ref which we can access
    return {
      focus: activate,
    };
  });
  ///
  ///
  <input
        ref={inputRef}
  ////
  ////
});

```

```js
const Login = (props) => {
  //////////////////////////
  /////////////////////
    const emailInputRef = useRef();
    const passwordInputRef = useRef();
    const submitHandler = (event) => {
    event.preventDefault();
    if (formIsValid) {
      authCtx.onLogin(emailState.value, passwordState.value);
    } else if (!emailIsValid) {
      emailInputRef.current.focus(); //here focus is not dom ele manipulation but the function focus which we got through useImperative
    } else {
      passwordInputRef.current.focus();
    }
  };
}
///////////////////////
//////////////////////////
return (
    <Card className={classes.login}>
      <form onSubmit={submitHandler}>
        <Input
          ref={emailInputRef} //we are passing ref as prop which is not normally possible without useImperative which allows us to send refs
```

---

# UseCallback()

- every time a page rerenfers the component functions are created again
- so to stop the rerendering we use `useCallBack

```js
const dispense = (candy) => {
  setCandies((allCandies) => allCandies.filter((c) => c !== candy));
};
```

```js
const dispense = React.useCallback((candy) => {
  setCandies((allCandies) => allCandies.filter((c) => c !== candy));
}, []);
```

- this renders only once unless its called and the function doesnt change
- if you want to change function when some variables change then add those variables to dependency array

---

# useMemo

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

- memorizedValue doesnt get rerendered unless you a or b changes
- if there are no ele in dependency array the memo only renders once in the start

---

# `using fetch in react`

```js
function App() {
  const [movies,setMovies]=useState([])
  function fetchMoviesHandler() {
    fetch("https://swapi.dev/api/films")
    .then((resp) => {
      return resp.json();
    }).then(data => {
      const transformedMovie = data.results.map(movieData => {
        return {
          id: movieData.episode_id,
          title: movieData.title,
          openingText: movieData.opening_crawl,
          releaseDate: movieData.release_date
        };
      });
      setMovies(transformedMovie);
    })
  }
```

resp.json() contains the body of api data which we further transform for our use

- we can use async awaits too as shown below

```js
async function fetchMoviesHandler() {
  const response = await fetch("https://swapi.dev/api/films");
  const data = await response.json();
  const transformedMovie = data.results.map((movieData) => {
    return {
      id: movieData.episode_id,
      title: movieData.title,
      openingText: movieData.opening_crawl,
      releaseDate: movieData.release_date,
    };
  });
  setMovies(transformedMovie);
}
```

```js
if (!response.ok) {
  throw new Error("something went wrong");
}
```

---

# using useEffect so that fetchMoviesHandler runs on first render

```js
useEffect(() => {
  fetchMoviesHandler();
}, [fetchMoviesHandler]);
```

- here dependency is fetchMoviesHandler but this function changes everytime it renders as the component rerenders so we useCallBack

```js
const fetchMoviesHandler = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    ///////////////////
    ////////////////
    /////code///////
    /////////////
    /////////////
    }
    setIsLoading(false);
  },[]);
```

- useeffect renders only once in the start then doesnt as function doesnt change

---

# using firebase

```js
//get request from our firebase database which gets our data
const response = await fetch(
  "https://react-http-6b4a6.firebaseio.com/movies.json"
);
//the data will be in the form of object with keys which point to each object entry we need
const data = await response.json();

const loadedMovies = [];

for (const key in data) {
  loadedMovies.push({
    id: key,
    title: data[key].title,
    openingText: data[key].openingText,
    releaseDate: data[key].releaseDate,
  });
}
```

`Handling post request in fetch`

```js
async function addMovieHandler(movie) {
  const response = await fetch(
    "https://react-practice-29af1-default-rtdb.firebaseio.com/movies.json",
    {
      method: "POST",
      body: JSON.stringify(movie), //convert the js object to string
      headers: {
        "Content-Type": "application/json",
      },
    }
  );
  const data = await response.json();
  console.log(data);
}
```

where movie is from
`AddMovie.js`

```js
//use useState for getting entries for new input
function AddMovie(props) {
  const titleRef = useRef('');
  const openingTextRef = useRef('');
  const releaseDateRef = useRef('');

  function submitHandler(event) {
    event.preventDefault();

    // could add validation here...

    const movie = {
      title: titleRef.current.value,
      openingText: openingTextRef.current.value,
      releaseDate: releaseDateRef.current.value,
    };

    props.onAddMovie(movie);  //lifts up to APP.js
  }
```
---
# custom hooks
```js
import { useState } from 'react';
//requestConfig is the data and applyData is a function
const useHttp = (requestConfig, applyData) => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
    const sendRequest = useCallback(async (requestConfig, applyData) => {
    setIsLoading(true);
    setError(null);
    try {
      const response = await fetch(requestConfig.url, {
        method: requestConfig.method ? requestConfig.method : 'GET',//if the input did not mention method
        headers: requestConfig.headers ? requestConfig.headers : {}, //did not mention headers
        body: requestConfig.body ? JSON.stringify(requestConfig.body) : null, //if it is a get request the user oesnt mention body
      });

      if (!response.ok) {
        throw new Error('Request failed!');
      }

      const data = await response.json();
      applyData(data); //add data to function
    } catch (err) {
      setError(err.message || 'Something went wrong!');
    }
    setIsLoading(false);
  }, []);

  return {
    isLoading,
    error,
    sendRequest, //we are returning all these which hcan be accessed by user
  };
};
```
> in this we return a function which has all state ,error `useStates` so we dont need to make one everytime we fetch or post data we t
APP.js
```js
import useHttp from './hooks/use-http';

function App() {
  const [tasks, setTasks] = useState([]);

  const { isLoading, error, sendRequest: fetchTasks } = useHttp();

  useEffect(() => {
    const transformTasks = (tasksObj) => {  //function that is passed
      const loadedTasks = [];

      for (const taskKey in tasksObj) {
        loadedTasks.push({ id: taskKey, text: tasksObj[taskKey].text });
      }

      setTasks(loadedTasks);
    };

    fetchTasks(
      { url: 'https://react-http-6b4a6.firebaseio.com/tasks.json' },
      transformTasks //passed as function
    );
  }, [fetchTasks]); //changes only when fetch tasks changes
```
```js
  const { isLoading, error, sendRequest: fetchTasks } = useHttp();
```
> we get a function and all isLoading and error are initialised in the function and the returned function is the the one we use when we need to do anything

`GET`
```js
fetchTasks(
      { url: 'https://react-http-6b4a6.firebaseio.com/tasks.json' },
      transformTasks //passed as function
    );
```
`Post`
```js
fetchTasks(
      { url: 'https://react-http-6b4a6.firebaseio.com/tasks.json' ,
        methods:"POST",
        headers:{"Content-Type": "application/json",},
        body: { //data to be added
          name:"STar Wars",
          episode:"23"      
        }
      },
      transformTasks //passed as function
    );
```
---
# adding functions as dependency in useffect
>The issue is that upon each render cycle, the function is redefined. React uses shallow object comparison to determine if a value updated or not. Each render cycle markup has a different reference. You can use useCallback to memoize the function though so the reference is stable and when usinf it in useEffect

---
# REDUX
![](img\contextdisadv.png)
![](D:\javascript\img\reduxcoreconc.png)
>is a appwie store
```
npm i redux
npm i  react-redux
```
>react can be used in any js so to connect react with redux we install the second package
```js
const redux = require('redux');
const counterReducer = (state={counter:0}, action) => {
    return {
        counter: state.counter + 1
    }
}
const store = redux.createStore(counterReducer)
console.log(store.getState())
const counterSubscriber = () => {
    console.log(store.getState());
}

store.subscribe(counterSubscriber)
store.dispatch({ type: 'incr' })
store.dispatch({type:'failed'})

```
>`subscribe()` => runs when ever store changes

>`dispatch()` determines the action one on the state of store

>`createStore(counterReducer)` =>>creates a store shose reducer functions is counterReducer


---
# using redux in react component
store component
```js
import { createStore } from 'redux';
const counterReducer = (state = { counter: 0 }, action) => {
  if (action.type === 'increment') {
    return {
      counter: state.counter + 1,
    };
  }
  if (action.type === 'decrement') {
    return {
      counter: state.counter - 1,
    };
  }

  return state;
};
const store = createStore(counterReducer);
export default store;
```
index.js
```js
import { Provider } from 'react-redux';////////import Provider
  <Provider store={store}>
    <App />
  </Provider>,
```
> # component where you use store

## `useSelector()`
```js
import { useSelector } from 'react-redux'; ////////////////
const Counter = () => {
  const counter = useSelector(state => state.counter);
/////////////
//////useSelector is used to get the state
```
## `useDispatch()`
```js
import {useSelector,useDispatch} from `react-redux`
const Counter = () => {
  const counter = useSelector(state => state.counter);
  const dispatch = useDispatch();
  //you get dispacth function of store
  const incrementHandler = () => {
    dispatch({ type: 'increment' });
  };

  const decrementHandler = () => {
    dispatch({ type: 'decrement' });
  };
  ////////
  //////////////////
  <button onClick={incrementHandler}>Increment</button>
  <button onClick={decrementHandler}>Decrement</button>
```
[article on dispatch and useselector](https://medium.com/@reireynoso?p=f7d8c7f75cdd)

---
> in redux reduxer the values you return dont get merged but overwrited
```js
const counterRed = (state = { counter: 0 ,show=true}, action) => {
    switch (action.type) {
        case "increment":
            return {
                counter: state.counter + 1,
                show:state.show
            };
        case "decrement":
            return {
                counter: state.counter - 1,
                show:state.show
            };
        case "increase":
            return {
                counter: state.counter + action.payload,
            };
        case "decrease":
            return {
                counter: state.counter - action.payload,
                show:state.show
            }
        default:
            return state;

    }
}
const store = createStore(counterRed);
export default store;
``` 
- here in case `increase` we dont return show so show will get removed from the state and we cant use it 

---
> [mutation of objects in js](https://alistapart.com/article/why-mutation-can-be-scary/)

---
# `reduxjs/toolki`
## `createSlice`
```js
import { createSlice, configureStore } from '@reduxjs/toolkit';

const initialState = { counter: 0, showCounter: true };

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment(state) {
      state.counter++;
    },
    decrement(state) {
      state.counter--;
    },
    increase(state, action) {
      state.counter = state.counter + action.payload;
    },
    toggleCounter(state) {
      state.showCounter = !state.showCounter;
    }
  }
});
const store =createStore(counterslice.reducers) ///////////.reducers
```
---
> using this we can see taht we dont have to wory about object mutation as createSlice makes a clone of our state and we can change any state and dont need to return many other states which dont change

---
# `configureStore()`
instead of the below we can use `configureStore` as shown when we work with multiple reducers
```js
const store =createStore(counterslice.reducers)//for one reducer
```
```js
import { createSlice, configureStore } from '@reduxjs/toolkit'; //////
const store = configureStore({
  reducer: {counterSlice.reducer}
});

export default store;
```
> here we can add more reducers for other states like user , modal and all others

> there are other options other than `configureStore` like `combineReducers`
---
> `accessing the reducer methods`
```js
const counterActions=counterSlice.actions ////
```
> we need to export the above as `counterSlice.actions.increase` gives us access to the increase function
```js

import { createSlice, configureStore } from '@reduxjs/toolkit'; //////
const store = configureStore({
  reducer: {counterSlice.reducer}
});
export const counterActions=counterSlice.actions
export default store;
```
the component accessing the store
```js
import { useDispatch } from 'react-redux';
import { counterActions } from '../store/index';
///////////////////////
/////////////////////
const Counter=()=>{
  /////////////
  ///////////
    const incrementHandler = () => {
    dispatch(counterActions.increment());
  };

  const increaseHandler = () => {
    dispatch(counterActions.increase(10));
  };
}
 return (
/////////////////////////////////////////////////////////////
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increase by 10</button>
        <button onClick={decrementHandler}>Decrement</button>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
      ///////////////////////////////////
  );
};
```
> as shown we need to access the counterActions to get functions of reducer

---
# adding mmultiple slices
```js
const initialAuthState = {
  isAuthenticated: false,
};

const authSlice = createSlice({
  name: 'authentication',
  initialState: initialAuthState,
  reducers: {
    login(state) {
      state.isAuthenticated = true;
    },
    logout(state) {
      state.isAuthenticated = false;
    },
  },
});

const store = configureStore({
  reducer: { counter: counterSlice.reducer, auth: authSlice.reducer },
});

export const counterActions = counterSlice.actions;
export const authActions = authSlice.actions;

export default store;
```
to get the value of a state or read
```js
const isAuth = useSelector((state) => state.auth.isAuthenticated);
```
> we can to operations inside reducer function and not just change state
but the function should be synchronous
`eg:`
```js
 reducers: {
    addItemToCart(state, action) {
      const newItem = action.payload;
      const existingItem = state.items.find((item) => item.id === newItem.id);
      state.totalQuantity++;
      if (!existingItem) {  //if item not already present
        state.items.push({
          id: newItem.id,
          price: newItem.price,
          quantity: 1,
          totalPrice: newItem.price,
          name: newItem.title
        });
      } else { //is already present
        existingItem.quantity++;
        existingItem.totalPrice = existingItem.totalPrice + newItem.price;
      }
    },
    removeItemFromCart(state, action) {
      const id = action.payload;
      const existingItem = state.items.find(item => item.id === id);
      state.totalQuantity--;
      if (existingItem.quantity === 1) {
        state.items = state.items.filter(item => item.id !== id);
      } else {
        existingItem.quantity--;
      }
    },
  },
});
```
---
# handling change in states to update firebase database
APP.js
```js
let isInitial = true;
function App() {
  const showCart = useSelector((state) => state.ui.cartIsVisible);
  const cart = useSelector((state) => state.cart);

  useEffect(() => {
    if (isInitial) {
      isInitial = false;
      return;
    }
    fetch('https://react-http-6b4a6.firebaseio.com/cart.json', {
      method: 'PUT',
      body: JSON.stringify(cart),
    });
  }, [cart]);

```
> so everytime the cart is updated with reducer the useselector automatically rerenders as said before due to which useEffect runs as its dependency has been changed
> isIntial is added so that it doesnt fetch data every time it is refreshed or on firsst render and only when there is change in cart
---
# react Router
```
npm i react-router-dom
```
- `Switch`  //obsolete
- `Link`
- `Route`
[link](https://reactrouter.com/docs/en/v6/upgrading/v5)
```js
        <Switch>
          <Route path='/welcome'>
            <Welcome />
          </Route>
          <Route path='/products' exact>
            <Products />
          </Route>
          <Route path='/products/:productId'>
            <ProductDetail />
          </Route>
        </Switch>
```
> you have to add exact as react renders the first one that matches so add exact

> Switch makes it so that only one gets rendered
```js
    <section>
      <h1>The Products Page</h1>
      <ul>
        <li>
          <Link to='/products/p1'>A Book</Link>
        </li>
        <li>
          <Link to='/products/p2'>A Carpet</Link>
        </li>
        <li>
          <Link to='/products/p3'>An Online Course</Link>
        </li>
      </ul>
    </section>
```
> use Link to go to other pages rather than <a>

---
# `BrowserRouter`
```js
import { BrowserRouter } from 'react-router-dom';
  <BrowserRouter>
    <App />
  </BrowserRouter>,
```
---
# `Redirect`
```js
      <Route path='/' exact>
        <Redirect to='/quotes'></Redirect>
      </Route>
```
---
# Nested Routes
app.js
```js
      <Route path='/quotes/:quoteId'>
      <QuoteDetail />
      </Route>
```
QuoteDetail.js
```js
<Route path={`/quotes/${params.quoteId}/comments`}>
  <Comments/>
</Route>
```
---
# Default Props
```js
const Rating = ({ value, text, color }) => {
//
//
///
/////
//////
};
Rating.defaultProps = {
  color: "#f8e825",
};  //we create an extra pop color and initialise it
```
# Proptypes
```js
Rating.propTypes = {  //adding rules to the props
    value: PropTypes.number.isRequired,
    text: PropTypes.string.isRequired,
    color:PropTypes.string,
}
```

---