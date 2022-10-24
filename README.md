# LearnJSAsync
Some simple note for beginners to better understand asynchronous in Javascript

<h1>1. SYNCHRONOUS</h1>
The program goes through each line of code, one by one. It has to wait for a line to finish before going to the next line

![image](https://user-images.githubusercontent.com/62002249/197486847-296988f7-a9b1-4a8e-9db9-446d39f96f25.png)

Example

```
const name = 'John';
const greeting = 'Hello my name is ' + name;
console.log(greeting);
```

Even if we call a function, it is still synchronous. Because the caller has to wait for the callee (function) to finish before it can continue its work

```
function greeting(name){
    return 'Hello my name is ' + name;
}

const name = 'John';
const sentence = greeting(name);
console.log(sentence);
```

**THIS LEADS TO A PROBLEM: What if a synchronous function takes too long to finish?**
For example
- LineA;
- LineB; (take a very long time)
- LineC; (will have to wait for B to finish before it can execute)

The code below use a very insufficient algorithm to generate primes. This lead to the program having to wait for an amount of time before it can execute the next line (output finish)

```
MAX_PRIME = 1000000

function isPrime(n) {
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return n > 1;
}

const random = (max) => Math.floor(Math.random() * max);

function generatePrimes(quota) {
  const primes = [];
  while (primes.length < quota) {
    const candidate = random(MAX_PRIME);
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }
  return primes;
}

// Print 'Start'
console.log('Start')
// This takes a very long time to finish
const primes = generatePrimes(1000000);
// Print 'Finished'
console.log('Finished!')
```

<h1> 2. ASYNCHRONOUS </h1>
To handle the problem with time-consuming synchronous code, we can use asynchronous.
The two most used methods are **Promise** and **Async / Await**

How asynchronous works:
- Start a long-running operation by calling a function.
- Have that function start the operation and return immediately, so that our program can still be responsive to other events.
- Notify us with the result of the operation when it eventually completes.
![image](https://user-images.githubusercontent.com/62002249/197490574-81288a1d-6bb2-47e3-b2d5-bf7a81b54bf8.png)

<h2> Promise </h2>

We will use ```fetch()``` to demonstrate asynchronous. Example
```
const fetchPromise = fetch('https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');

console.log(fetchPromise);

fetchPromise.then((response) => {
  console.log(`Received response: ${response.status}`);
});

console.log("Started request…");
```

The code above:
- Call the fetch() API, and assigning the return value to the fetchPromise variable
- Immediately after, logging the fetchPromise variable. This should output something like: ```Promise { <state>: "pending" }```, telling us that we have a Promise object, and it has a state whose value is "pending". The "pending" state means that the fetch operation is still going on.
- When (and if) the fetch operation succeeds, the promise will execute anything inside its ```.then()```, in this case we print out the server's response.
- Print a message that we have started the request.

The output should looks like this
```
Promise { <state>: "pending" }
Started request…
Received response: 200
```



As we have already known, these code block will have different outputs because they are synchronous code

```
console.log('First')
console.log('Second')
```
and

```
console.log('Second')
console.log('First')
```

However with ```fetch()``` , we clearly see that although ```console.log("Started request...");``` is below ```fetch()``` , it output is logged before the ```fetch()``` output. This is because ```fetch()``` takes longer to finish its task.

![image](https://user-images.githubusercontent.com/62002249/197495188-001d0787-0649-4fdf-86b4-dd52b92c8ebc.png)



<h2> Async / Await </h2>
