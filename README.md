# LearnJSAsync
Some simple note for beginners to better understand asynchronous in Javascript

<h1>1. SYNCHRONOUS</h1>
The program goes through each line of code, one by one. It has to wait for a line to finish before going to the next line

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

**THIS LEAD TO A PROBLEM: What if a synchronous function takes too long to finish?**
For example
LineA;
LineB; (take a very long time)
LineC; (will have to wait for B to finish before it can execute)
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
