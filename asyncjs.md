# Asynchronous JS
## Promises
---
``` javascript
const fakeRequest = (url) => {
    return new Promise((resolve, reject) => {
        const rand = Math.random();
        setTimeout(() => {
            if (rand < 0.7) {
                resolve('YOUR FAKE DATA HERE');
            }
            reject('Request Error!');
        }, 1000)
    })
}

fakeRequest('/dogs/1')
    .then((data) => {
        console.log("DONE WITH REQUEST!")
        console.log('data is:', data)
    })
    .catch((err) => {
        console.log("OH NO!", err)
    })
```
### Example 
``` javascript
const delayedColorChange = (color, delay) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            document.body.style.backgroundColor = color;
            resolve();
        }, delay)
    })
}


delayedColorChange('red', 1000)
    .then(() => delayedColorChange('orange', 1000))
    .then(() => delayedColorChange('yellow', 1000))
    .then(() => delayedColorChange('green', 1000))
    .then(() => delayedColorChange('blue', 1000))
    .then(() => delayedColorChange('indigo', 1000))
    .then(() => delayedColorChange('violet', 1000))
```
---
## Nested Callbacks
``` javascript
const fakerequest = (str)=>{
    return new Promise((resolve,reject) => {
        const rand = Math.random();
        setTimeout(()=>{
        if(rand>0.5){
            resolve(str);
        }
        reject(str); 
    },1000);
  })
}
fakerequest('myHOmePage')
    .then((data)=>{
        console.log('SUCCESS!!! '+ data);
        return fakerequest('page1')
    })
    .then((data)=>{
        console.log('SUCCESS!!! '+ data);
        return fakerequest('page2')
    })
    .then((data)=>{
        console.log('SUCCESS!!! '+ data);
        return fakerequest('page3')
    })
    .then((data)=>{
        console.log('SUCCESS!!! '+ data);
        return fakerequest('page4')
    })
    .then((data)=>{
        console.log('SUCCESS!!! '+ data);
    })
    .catch((data)=>{
        console.log('FAIL!!! '+ data);// stops when any of the above fails
    });

```
here if we fail to get page 2<br>
output will be:<br>
```
SUCCESS!!! myHOmePage
SUCCESS!!! page1
FAIL!!! page2
```
---
## Asychronous functions<br>
Asynch functions always return a promise<br>
Resolved: return <br> 
Rejected: throw
``` javascript
const login = async (username, password) => {
    if (!username || !password) throw 'Missing Credentials'
    if (password === 'corgifeetarecute') return 'WELCOME!'
    throw 'Invalid Password'
}

login('todd', 'corgifeetarecute')
    .then(msg => {
        console.log("LOGGED IN!")
        console.log(msg)
    })
    .catch(err => {
        console.log("ERROR!")
        console.log(err)
    })
```
---
## Await
await makes sure that another promise doesnt run until the present one is completed.<br>
only works in async functions
``` javascript
const delayedColorChange = (color, delay) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            document.body.style.backgroundColor = color;
            resolve();
        }, delay)
    })
}

async function rainbow(){
    await delayedColorChange('red', 1000);
    await delayedColorChange('orange', 1000);
    await delayedColorChange('yellow', 1000);
    await delayedColorChange('green', 1000);
    await delayedColorChange('blue', 1000);
    await delayedColorChange('indigo', 1000);
    await delayedColorChange('violet', 1000);
    return 'rainbow done';
}
rainbow().then((data) => console.log(data));;
```
---
## XMLHTTPRequest
``` javascript
const req = new XMLHttpRequest();
req.onload = function(){
    console.log("request success")
    const data = JSON.parse(this.responseText);
    console.log('price: ',data.ticker.price)
}
req.onerror = ()=>{
    console.log("error");
}
req.open('GET','https://api.cryptonator.com/api/ticker/btc-usd');
req.send();
```
---
## FETCH API
```javascript
fetch('https://api.cryptonator.com/api/ticker/btc-usd')//returns a promise
    .then((res)=>{
        return res.json()//returns a promise with data in json format or response converted to json
    })//cant put ';' or error occors
    .then((data)=>{
        console.log("price: ",data.ticker.price);
    })
    .catch((e)=>{
        console.log("error")
    });
```
### Using await
``` javascript
async function FetchBitcoin(){
    try{ //to handle errors
    const res = await fetch('https://api.cryptonator.com/api/ticker/btc-usd1');//returns a promise
    const data = await res.json();
    console.log('price: ',data.ticker.price);
    }catch(e){
        console.log(e);
    }
}
setInterval(FetchBitcoin,10000);
```
---
## AXIOS
``` javascript
async function FetchBitcoin(){
    try{
    const res = await axios.get('https://api.cryptonator.com/api/ticker/btc-usd')
    console.log(res.data.ticker.price); //json is in data 
    }catch(e){
        console.log(e);
    }
}
FetchBitcoin();
```
Script file for axios must be above our script

---
## returning values in async instead of promise
```javascript
async function FetchBitcoin(){
    try{
    const res = await axios.get('https://api.cryptonator.com/api/ticker/btc-usd')
    return res.data.ticker.price;
    }catch(e){
        return 'No Data'
    }
}
async function printData(){
    const str =  await FetchBitcoin(); //must use await or we get promise as return
    console.log('price: ',str);
}
printData();
```
here  the calling function must be async or doesnt work