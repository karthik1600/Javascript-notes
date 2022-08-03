```js
import React from 'react';
import ReactDom from 'react-dom'
//use className instead of class
//have to have a clossind tag for every tag in html <img src=' '> =><img src=' '/>
function Greeting(){
  return (<h1>this is john</h1>);
} 
  // const Greeting = () =>{
  //   return <h1>this is john</h1>;
  // }
// function Singleel(){     //error as we are return ing more tahn one component
//      return( 
//        <h1>hello</h1>
//        <b>kello</b>
//      );
// }
function Singleel(){    //wrap it in a div to mamke it single component
     return( 
       <div>
       <h1>hello</h1>
       <b>kello</b>
       </div>
     );
}
ReactDom.render(<Greeting/>,document.getElementById('root')
);
//ReactDom.render(<Greeting></Greeting>,document.getElementById('root'));
```
point to note : jsx uses camelcase abcDbe 

---
nested components
```js
function Greeting(){
  return (
      <div>
         <h1>this is john</h1>
         <p>this is person</p>
         </div>
         );
} 
```
```js
function Greeting(){
  return (
      <div>
         <Name1 />
         <Person />
      </div>
         );
} 
const Person=()=> <p>this is person</p>;
const Name1 =()=> {
    return <h1>this is john</h1>;
}
```

---
## `how to use javascript in jsx`
```js
function Greeting(){
  return (
      const auth='john';
      <div>
         <h1>{auth}</h1>
         <h1>{6+6}</h1>
        {/*<p>{let x=9}</p>         error  */}
         </div>
         );
} 
```
---
## `importing css`
```js
import './index.css';
```
---

# PROPS
```js
function BookList(){
    return (
        <section>
            <Book
                name='karthik'
                age= '21'
            />
            <Book
            name='rohan'
            age = '15'
            />
        </section>
    );
};
function Book(props){
  return (
      <div>
         <h1> name={props.name}</h1>
         <p> age ={props.age} </p>>
      </div>
         );
} 
//or
// function Book(props){
//     const {name,age}=props
//   return (
//       <div>
//          <h1> name={name}</h1>
//          <p> age ={age} </p>>
//       </div>
//          );
// } 
//or
// function Book({name,age}){
//   return (
//       <div>
//          <h1> name={name}</h1>
//          <p> age ={age} </p>>
//       </div>
//          );
// } 
```
---
# `PROP children`
```js
function BookList(){
    return (
        <section>
            <Book
                name='karthik'
                age= '21'
            >
              <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eveniet voluptas maxime atque vero nostrum. Maiores soluta eos itaque facere in.</p>
              </Book>
            <Book
            name='rohan'
            age = '15'
            />
        </section>
    );
};
function Book(props){
  return (
      <div>
         <h1> name={props.name}</h1>
          {props.children}
         <p> age ={props.age} </p>
      </div>
         );
} 
```
- only first book has children
- anything between opening and closing of a component

---
## `list in react`
```js
const books = [
 {
  id: 1,
  img:
   'https://images-na.ssl-images-amazon.com/images/I/81eB%2B7%2BCkUL._AC_UL200_SR200,200_.jpg',
  title: 'I Love You to the Moon and Back',
  author: 'Amelia Hepworth',
 },
 {
  id: 2,
  img:
   'https://images-na.ssl-images-amazon.com/images/I/71aLultW5EL._AC_UL200_SR200,200_.jpg',
  title: 'Our Class is a Family',
  author: 'Shannon Olsen',
 },
 {
  id: 3,
  img:
   'https://images-na.ssl-images-amazon.com/images/I/71e5m7xQd0L._AC_UL200_SR200,200_.jpg',
  title: 'The Vanishing Half: A Novel',
  author: 'Brit Bennett',
 },
];
function BookList(){
    return (
        <section>
          {books.map((book)=>{
            return <Book book={book}></Book>
          })}
        </section>
    );
};
function Book(props){
  const {id,img,title,author}=props.book
  return (
      <div>
         <h1> name={title}</h1>
         <img src={img}></img>
         <p> author ={author} </p>
         <p> id ={id} </p>

      </div>
         );
} 
//or
// function BookList(){
//     return (
//         <section>
//           {books.map((book)=>{
//             return <Book {...book}></Book>  //spreading
//           })}
//         </section>
//     );
// };
// function Book(props){
//   const {id,img,title,author}=props
//   return (
//       <div>
//          <h1> name={title}</h1>
//          <img src={img}></img>
//          <p> author ={author} </p>
//          <p> id ={id} </p>

//       </div>
//          );
// } 
```

---
## `key prop`
here
```js
return <Book key={books.id} book={book}></Book>
```
---
## `event handlers`
```js
  const {id,img,title,author}=props.book
  const clickHandler=()=>{
    alert('hello')
  }
  return (
      <div>
         <h1> name={title}</h1>
         <img src={img}></img>
         <p> author ={author} </p>
         <p> id ={id} </p>
        <button type="button" onClick={clickHandler}>reference</button>
      </div>
         );
} 
```
- when you pass arguments in event handler you need to make the function a return of another function
```js
  const {id,img,title,author}=props.book
  const clickHandler=(author)=>{
    alert(author)
  }
  return (
      <div>
         <h1> name={title}</h1>
         <img src={img}></img>
         <p> author ={author} </p>
         <p> id ={id} </p>
        <button type="button" onClick={()=>clickHandler(author)}>reference</button>
      </div>
         );
} 
```
---
## `react fragments`
```js
  return (
    <React.Fragment>
      <h2>{title}</h2>
      <button type="button" className="btn" onClick={handleClick}>
        change title
      </button>
    </React.Fragment>
  );
};
```
no need for {} when using js

---
## useState
```js
import React, { useState } from 'react';
// starts with use
// component must be uppercase
// invoke inside function/component body
// don't call hooks conditonally
useState('random')   // => return array ['random' , func]
console.log(useState('random')[0])    //=> 'random'
console.log(useState('random')[1])    //=> function
const [text,setText]=useState('random')
setText('changed') // => changes text from random to 'changed'
``` 
`using useState to change a text on click`
```js
const UseStateBasics = () => {
  const [text, setText] = useState('random title')
  const handleClick = () => {
    setText('changed form random ')
  }
  return (
    <React.Fragment>
      <h1>{text}</h1>
      <button className="button" onClick={handleClick}>change</button>
  </React.Fragment>
    )
};
```
## `useState in array`
```js
const data = [
  { id: 1, name: 'john' },
  { id: 2, name: 'peter' },
  { id: 3, name: 'susan' },
  { id: 4, name: 'anna' },
];
const UseStateArray = () => {
  const [people, setPeople] = React.useState(data);
  return (
    <>
    {
      people.map((person) => {
      const {id,name}=person;
      return(
      <div key={id} className='item'>
        <h4>{name}</h4>
    </div>
      );
       })
      }
      <button className='btn' onClick={()=>setPeople([])}>clear item</button>
  </>
  );
};
```
above we change the useState to empty thereby clearing the people
## `removing a single item from array`
```js
const UseStateArray = () => {
  const [people, setPeople] = React.useState(data);
  const removeItem = (id) => {
    let newPeople=people.filter((person)=> person.id!==id) //removes the data with id
    setPeople(newPeople);
  }
  return (
    <>
    {
      people.map((person) => {
      const {id,name}=person;
      return(
      <div key={id} className='item'>
          <h4>{name}</h4>
          <button onClick={()=>removeItem(id)}>remove</button>
    </div>
      );
       })
      }
      <button className='btn' onClick={()=>setPeople([])}>clear item</button>
  </>
  );
};
```
---
## `use state on objects`
```js
const UseStateObject = () => {
  const [person, setPerson] = useState({
    name: 'peter',
    age: 24,
    message: 'random message',
  });

  // const [name,setName] = useState('peter')
  // const [age,setAge] = useState(24)
  // const [message,setMessage] = useState('random message')

  const changeMessage = () => {
    setPerson({ ...person, message: 'hello world' });
    // setPerson({ ...person, age: 43, message: "hello its changed" }); changes age and message
  };
  return (
    <>
      <h3>{person.name}</h3>
      <h3>{person.age}</h3>
      <h4>{person.message}</h4>
      <button className='btn' onClick={changeMessage}>
        change message
      </button>
    </>
  );
};
```
---
## `use state with setTimeout`
```js
const UseStateCounter = () => {
  const [value, setValue] = useState(0);

  const reset = () => {
    setValue(0);
  };

  const complexIncrease = () => {
    setTimeout(() => {
      // setValue(value + 1);
      setValue((value) => value + 1);
    }, 2000);
  };
```
- in above example we cannot do `setValue(value + 1);` as it doesnt get updated when timeout so we do the below one

---
# `useEffect`
```js
import React, { useState, useEffect } from "react";
// by default runs after every re-render
// cleanup function
// second parameter
const UseEffectBasics = () => {
  const [value, setValue] = useState(0);
  useEffect(() => {
    if(value>0)  //only runs when value greater than 1
    document.title = `new messages(${value})`;
  });
  console.log("render");
  return (
    <>
      <h1>{value}</h1>
      <button className="btn" onClick={() => setValue(value + 1)}>
        click me
      </button>
    </>
  );
};
```
- use to change things outsisde component
- above we change the title of page everytime it re renders
---
`second parameter`
```js
const UseEffectBasics = () => {
  const [value, setValue] = useState(0);
  useEffect(() => {
    console.log('calleffect')
    if(value>0)
    document.title = `new messages(${value})`;
  },[value]);//second parameter
```
- useEffect only works on reload or when vaalue changes else if ther is no change in value and page rerender effect doesnt take place

## `cleanup`
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
- everytime the page renders when window size increases a new eventlistner is added so we need to delete one before adding another
`eg`
```js
const UseEffectFetchData = () => {
  const [users, setUsers] = useState([]);
  const getUsers = async () => {
    const response = await fetch(url);
    const users = await response.json();
    setUsers(users);
  };
  useEffect(() => {
    getUsers();
  }, []);
  return (
    <>
      <h3>github users</h3>
      <ul className="users">
        {users.map((user) => {
          const { id, login, avatar_url, html_url } = user;
          return (
            <li key={id}>
              <img src={avatar_url} alt={login} />
              <div>
                <h4>{login}</h4>
                <a href={html_url}>profile</a>
              </div>
            </li>
          );
        })}
      </ul>
    </>
  );
};
```
---
# `conditional returns`
```js
const MultipleReturns = () => {
  const [loading, setLoading] = useState(true)
  const [isError, setIsError] = useState(false)
  const [user, setUser] = useState('defaul user')
  useEffect(() => {
    fetch(url)
      .then((resp) => {
        if (resp.status >= 200 && resp.status <= 399) {
          return resp.json();
        }
        else {
          setLoading(false);
          setIsError(true);
        }
      })
      .then((user) => {
        const { login } = user;
        setUser(login);
        setLoading(false)
      })
      .catch((error) => console.log(error));
  },[])
  if (loading) {
    return <h2>loading</h2>;
  }
  if (isError) {
    return <h2>Error...</h2>
  }
  return <h2>multiple returns</h2>;
};
```
- here when we get data loading becomes false and we get normal output
- if there is an error we make loading false and error true so we are taken to error page

---
`terenary operator`
```js
{isError ? <h1>Error....</h1> : <h1>There is no Error</h1>}
```

---
# `hide or show component`
```js
const ShowHide = () => {
  const [show, setShow] = useState(false);
  return (
    <>
      <button className="btn" onClick={() => setShow(!show)}>
        show/hide
      </button>
      {show && <Item />}
    </>

  )
};
const Item = () => {

  return (
    <div style={{ marginTop: '2rem' }}>
      <h1>Window</h1>
      <h2>size:</h2>
    </div>
  )
}
```
---
# `forms`
```js
const ControlledInputs = () => {
  const [firstName, setFirstname] = useState("");
  const [email, setEmail] = useState("");
  const [people, setPeople] = useState([]);
  const handleSubmit = (e) => {
    e.preventDefault();
    if (firstName && email) {    //if both fields are filled
      const person = {
        firstName: firstName,
        email: email,
      };
      console.log(person)
      setPeople((people) => {  //previous people 
        return [...people, person]; //adding the person to array
      });
      setEmail('')
      setFirstname('')
      console.log(people);
    }
    else {
      console.log('empty')
    }

  };
  return (
    <>
      <article>
        <form className="form" onSubmit={handleSubmit}>


<input type="text"
  id="firstName"
  name="firstName"
  value={firstName}
  onChange={(e)=>setFirstname(e.target.value)}
/>
```
## ` multiple inputs
```js
const ControlledInputs = () => {
  const [person, setPerson] = useState({ firstName: '', email: '', age: '' });
  const [people, setPeople] = useState([]);
  const handleChange = (e) => {
    const name = e.target.name;  //name gets us the property which has changed
    const value = e.target.value;
    setPerson({ ...person, [name]: value });
  };
  const handleSubmit = (e) => {
    e.preventDefault();
    if (person.firstName && person.email && person.age) {
      const newPerson = { ...person, id: new Date().getTime().toString() };
      setPeople([...people, newPerson]);
      setPerson({ firstName: '', email: '', age: '' });
    }
  };
// form part of input

<div className='form-control'>
            <label htmlFor='email'>Email : </label>
            <input
              type='email'
              id='email'
              name='email'
              value={person.email}
              onChange={handleChange}
            />
</div>
<div className='form-control'>
  <label htmlFor='age'>Age : </label>
  <input
    type='number'
    id='age'
    name='age'  //name gives us the property
    value={person.age}
    onChange={handleChange}
  />
```
---
# `useRef`
```js
// preserves value
// DOES NOT trigger re-render
// target DOM nodes/elements

const UseRefBasics = () => {
  const refContainer = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(refContainer.current.value);
  };
  // useEffect(() => {                //no need for this
  //   console.log(refContainer.current);
  // });

  return (
    <>
      <form className='form' onSubmit={handleSubmit}>
        <div>
          <input type='text' ref={refContainer} />      //we automatically get changed value after submitting
        </div>
        <button type='submit'>submit</button>
      </form>
    </>
  );
};
```
---
## useReduce
```js
import { reducer } from './reducer';
const defaultState = {
  people: [],
  isModalOpen: false,
  modalContent: '',
};
const Index = () => {
  const [name, setName] = useState('');
  const [state, dispatch] = useReducer(reducer, defaultState); //reducer is the function which is action done on state
  const handleSubmit = (e) => {
    e.preventDefault();
    if (name) {
      const newItem = { id: new Date().getTime().toString(), name };
      dispatch({ type: 'ADD_ITEM', payload: newItem });
      setName('');
    } else {
      dispatch({ type: 'NO_VALUE' });
    }
  };
  const closeModal = () => {
    dispatch({ type: 'CLOSE_MODAL' });
  };
```
```js
export const reducer = (state, action) => {   //state = state before action and action are the actions below which have 2 datas which are type and payload which is value
  if (action.type === 'ADD_ITEM') {
    const newPeople = [...state.people, action.payload];
    return {
      ...state,
      people: newPeople,
      isModalOpen: true,
      modalContent: 'item added',
    };
  }
  if (action.type === 'NO_VALUE') {
    return { ...state, isModalOpen: true, modalContent: 'please enter value' };
  }
  if (action.type === 'CLOSE_MODAL') {
    return { ...state, isModalOpen: false };
  }
  if (action.type === 'REMOVE_ITEM') {
    const newPeople = state.people.filter(
      (person) => person.id !== action.payload
    );
    return { ...state, people: newPeople };
  }
  throw new Error('no matching action type');
};
```
```js
return (
    <>
      {state.isModalOpen && (  // we do this so after modal i shown it automatically closes with the func closeModal
        <Modal closeModal={closeModal} modalContent={state.modalContent} />
      )}
      <form onSubmit={handleSubmit} className='form'>
        <div>
          <input
            type='text'
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
        </div>
        <button type='submit'>add </button>
      </form>
      {state.people.map((person) => {
        return (
          <div key={person.id} className='item'>
            <h4>{person.name}</h4>
            <button
              onClick={() =>
                dispatch({ type: 'REMOVE_ITEM', payload: person.id }) // dispatch
              }
            >
              remove
            </button>
          </div>
        );
      })}
    </>
  );
```

## `prop drilling`
```js
const PropDrilling = () => {
  const [people, setPeople] = useState(data);
  const removePerson = (id) => {         //////////////////////
    setPeople((people) => {
      return people.filter((person) => person.id !== id);
    });
  };
  return (
    <section>
      <h3>prop drilling</h3>
      <List people={people} removePerson={removePerson} />  /////////
    </section>
  );
};

const List = ({ people, removePerson }) => {           //////////////
  return (
    <>
      {people.map((person) => {
        return (
          <SinglePerson
            key={person.id}
            {...person}
            removePerson={removePerson}      ////////////////////////
          />
        );
      })}
    </>
  );
};

const SinglePerson = ({ id, name, removePerson }) => { /////////////
  return (
    <div className='item'>
      <h4>{name}</h4>
      <button onClick={() => removePerson(id)}>remove</button>
    </div>
  );
};

```
 - here we drilling the removePerson to list and then further drilling fromm List to singleperson  so that the function can be used here

 ---
# `useContext`
 ```js
 const PersonContext = React.createContext();  //
// two components - Provider, Consumer

const ContextAPI = () => {
  const [people, setPeople] = useState(data); 
  const removePerson = (id) => {
    setPeople((people) => {
      return people.filter((person) => person.id !== id);
    });
  };
  return (    ////////////////////////////////////////
    <PersonContext.Provider value={{ removePerson, people }}>
      <h3>Context API / useContext</h3>
      <List />
    </PersonContext.Provider>
  );////////////////////////
};

const List = () => {
  const mainData = useContext(PersonContext);///////////////////
  console.log(mainData);
  return (
    <>
      {mainData.people.map((person) => {                /////////
        return <SinglePerson key={person.id} {...person} />;
      })}
    </>
  );
};

const SinglePerson = ({ id, name }) => {
  const { removePerson } = useContext(PersonContext);  ////////////

  return (
    <div className='item'>
      <h4>{name}</h4>
      <button onClick={() => removePerson(id)}>remove</button>
    </div>
  );
};

```
- no need for drilling aand anything can be accesed at any layer between context
- here `removePerson` is 2 layer down between the context

---

# `creating a custom hook`
```js
import { useState, useEffect, useCallback } from 'react';

export const useFetch = (url) => {          ////////      need to use useto create custom hook   
  const [loading, setLoading] = useState(true);
  const [products, setProducts] = useState([]);

  const getProducts = useCallback(async () => {
    const response = await fetch(url);
    const products = await response.json();
    setProducts(products);
    setLoading(false);
  }, [url]);

  useEffect(() => {
    getProducts();
  }, [url, getProducts]);
  return { loading, products };         /////////////////////
};
```
- above function takes a url and uses useState and useEffect on using a single hook as shown below
```js
const { loading, products } = useFetch(url)
```

---
# `proptypes`
```js
import PropTypes from 'prop-types'; ///////////////////
const Product = ({ image, name, price }) => {
  const url = image && image.url;
  return (
    <article className='product'>
      <img src={url || defaultImage} alt={name || 'default name'} />
      <h4>{name}</h4>
      <p>${price || 3.99}</p>
    </article>
  );
};

Product.propTypes = {      /////////////////////
  image: PropTypes.object.isRequired,
  name: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
};
// Product.defaultProps = {
//   name: 'default name',
//   price: 3.99,
//   image: defaultImage,
// };
```
- in the above some props may not be available so we can use proptypes like the above example
---
# `React Router`
```
npm install react-router-dom
```
```js
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
// pages
import Home from './Home';
import About from './About';
import People from './People';
import Error from './Error';
import Person from './Person';
// navbar
import Navbar from './Navbar';
const ReactRouterSetup = () => {
  return (
    <Router> //////////////////////////
      <Navbar />
      <Switch>
        <Route exact path='/'>///////////////////
          <Home />
        </Route>
        <Route path='/about'>
          <About />
        </Route>
        <Route path='/people'>
          <People />
        </Route>
        <Route path='/person/:id' children={<Person />}></Route>//////////////
        <Route path='*'>
          <Error />
        </Route>
      </Switch>
    </Router>
  );
};
```
- in the above we use `Switch` so only one of the paths are selected
- in the `'/person/:id'` id is param in person as shown below
```js
import { Link, useParams } from 'react-router-dom';///////
const Person = () => {
  const [name, setName] = useState('default name');
  const { id } = useParams();

  useEffect(() => {
    const newPerson = data.find((person) => person.id === parseInt(id));/////////////
    setName(newPerson.name);
  }, []);
  return (
    <div>
      <h1>{name}</h1>
      <Link to='/people' className='btn'>
        Back To People
      </Link>
    </div>
  );
};
```
- we import `useParams` to get parameter from link
- the param we get using this is string so we need to convert it first

```js
const Navbar = () => {
  return (
    <nav>
      <ul>
        <li>
          <Link to='/'>Home</Link>
        </li>
        <li>
          <Link to='/about'>About</Link>
        </li>
        <li>
          <Link to='/people'>People</Link>
        </li>
      </ul>
    </nav>
  );
};
```
---
# Projects
```js
const [readMore,setReadMore]=useState(true)
<p>
          {readMore ? info : `${info.substring(0, 200)}...`}
          <button onClick={() => setReadMore(!readMore)}>
            {readMore ? "show less" : "  read more"} //the name of button changes according to readmore value
          </button>
</p>
```
- to increase or decrease the length of anything  
```js
  const [index, setIndex] = useState(0);
  const { name, job, image, text } = people[index]
```
- for next and prev where index is the location of array data where we can change the info by changing index

```js
const allCategories = ['all', ...new Set(items.map((item) => item.category))];
```
- create a category set

---
# `Slider`
```js
  {people.map((person, personIndex) => {
          const { id, image, name, title, quote } = person;

          let position = 'nextSlide';
          if (personIndex === index) {
            position = 'activeSlide';
          }
          if (
            personIndex === index - 1 ||
            (index === 0 && personIndex === people.length - 1)
          ) {
            position = 'lastSlide';
          }

          return (
            <article className={position} key={id}>
              <img src={image} alt={name} className="person-img" />
              <h4>{name}</h4>
              <p className="title">{title}</p>
              <p className="text">{quote}</p>
              <FaQuoteRight className="icon" />
            </article>
          );
        })}
        <button className="prev" onClick={() => setIndex(index - 1)}>
          <FiChevronLeft />
        </button>
        <button className="next" onClick={() => setIndex(index + 1)}>
          <FiChevronRight />
        </button>
      </div>
    </section>
  );
```
css
```css
article {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: var(--transition);
}
article.activeSlide {
  opacity: 1;
  transform: translateX(0);
}
article.lastSlide {
  transform: translateX(-100%);
}
article.nextSlide {
  transform: translateX(100%);
}
```
---
```js
setText(data.slice(0, amount));
```
- to select a range of values from array
---
## `copy to clipboard`
```js
  const [alert, setAlert] = useState(false)
    useEffect(() => {         //so that alert will be shown then hides itself after 3s
    const timeout = setTimeout(() => {
      setAlert(false)
    }, 3000)
    return () => clearTimeout(timeout)
  }, [alert])
```
```js
    <article
      className={`color ${index > 10 && 'color-light'}`}
      style={{ backgroundColor: `rgb(${bcg})` }}
      onClick={() => {
        setAlert(true) //////////////////////
        navigator.clipboard.writeText(hexValue) //////////
      }}
    >
      <p className='percent-value'>{weight}%</p>
      <p className='color-value'>{hexValue}</p>
      {alert && <p className='alert'>copied to clipboard</p>}
    </article>
    {alert && <p className='alert'>copied to clipboard</p>}//////////
```
---
# `editing or adding new item using same form input`
```js
function App() {
  const [name, setName] = useState('');
  const [list, setList] = useState(getLocalStorage());
  const [isEditing, setIsEditing] = useState(false);
  const [editID, setEditID] = useState(null);
  const [alert, setAlert] = useState({ show: false, msg: '', type: '' });
  const handleSubmit = (e) => {
    e.preventDefault();
    if (!name) {
      showAlert(true, 'danger', 'please enter value');
    } else if (name && isEditing) {
      setList(
        list.map((item) => {
          if (item.id === editID) {
            return { ...item, title: name };
          }
          return item;
        })
      );
      setName('');
      setEditID(null);
      setIsEditing(false);
      showAlert(true, 'success', 'value changed');
    } else {
      showAlert(true, 'success', 'item added to the list');
      const newItem = { id: new Date().getTime().toString(), title: name };

      setList([...list, newItem]);
      setName('');
    }
  };

  const showAlert = (show = false, type = '', msg = '') => {
    setAlert({ show, type, msg });
  };
  const clearList = () => {
    showAlert(true, 'danger', 'empty list');
    setList([]);
  };
  const removeItem = (id) => {
    showAlert(true, 'danger', 'item removed');
    setList(list.filter((item) => item.id !== id));
  };
  const editItem = (id) => {
    const specificItem = list.find((item) => item.id === id);
    setIsEditing(true);
    setEditID(id);
    setName(specificItem.title);
  };
  useEffect(() => {
    localStorage.setItem('list', JSON.stringify(list));
  }, [list]);
```
```js
return (
    <section className='section-center'>
      <form className='grocery-form' onSubmit={handleSubmit}>
        {alert.show && <Alert {...alert} removeAlert={showAlert} list={list} />}

        <h3>grocery bud</h3>
        <div className='form-control'>
          <input
            type='text'
            className='grocery'
            placeholder='e.g. eggs'
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <button type='submit' className='submit-btn'>
            {isEditing ? 'edit' : 'submit'}
          </button>
        </div>
      </form>
      {list.length > 0 && (
        <div className='grocery-container'>
          <List items={list} removeItem={removeItem} editItem={editItem} />
          <button className='clear-btn' onClick={clearList}>
            clear items
          </button>
        </div>
      )}
    </section>
  );
```
---
## useNavigate
```js
import { useNavigate } from 'react-router-dom';
const navigate = useNavigate();
navigate('/home');
```
eg
```js
  const addToCartHandler = () => {
    navigate(`/cart/${id}?qty=${qty}`);
  };
```