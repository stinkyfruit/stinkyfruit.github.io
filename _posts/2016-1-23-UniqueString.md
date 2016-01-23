---
layout: post
title: Unique String
---

##Javascript Unique String Problem

So here's a little problem to get your Javascript brain juices flowing...


When given a string of characters, return a string with only the characters that are unique to the string. For example:

"aaaabbbbc" should return => "c"

"aabbccdeeff1123" should return => "d2"

and so on...

##Planning Approach

###Part 1

So first thing is first, you should loop through your string at one point or another. Looping through a string works the same as looping through an array, where you can access a certain property on the string by giving an index value(starting at 0). 

So if you wanted to access the 2nd character of the following string: "hello", you would say string[1] - the 2nd character is located at the 1st index spot(same as arrays)

###Part 2

You want to think of where and how you will want to store all unique values. So first thing that comes to my mind is an object. An object's lookup is performed in constant time, and you can easily rewrite over the values of the key, value pair. So for this problem, I will be using a storage object

###Part 3

When you've stored all of the unique values, you want to return the values in a string, and only the unique values


##Onto Coding

```javascript
/* creating the function */
var onlyUnique = function(string){
  
};
```
So we've created our function and we've passed in a string parameter placeholder. When we call our function, a string will be passed in as a parameter. 

Next step, we're going to initialize a return string and a storage object, like so:


```javascript
var onlyUnique = function(string){
  var returnString = '';
  var storageObject = {};
};
```
So now we want to loop through our string, with a simple for loop, and add our values into the storage object. Everytime we come across an object that already exists, it won't matter because the new value will just overwrite the old value. 

```javascript
var onlyUnique = function(string){
  var returnString = '';
  var storageObject = {};

  for(var i = 0; i < string.length; i++) {
    if(storageObject[string[i]]) {
      storageObject[string[i]]++
    } else {
      storageObject[string[i]] = 1; 
    }
  }
};
```

What we're doing in the for loop is checking to see if the string at value i exists as a key in the storage object. If it does, increment the value. If it doesn't, set the value to equal to 1. 


If we call the function and pass in the string, "aabbccdef", the storage object will look something like the following:

```javascript
storageObject = {
  a: 2,
  b: 2,
  c: 2,
  d: 1,
  e: 1,
  f: 1
}
```

If you notice, all of the unique values of the string have a value in the storageObject as 1; therefore, to return only the unique values, we will want to loop through the object, and return all of the keys that have a corresponding value of 1. 

It will look like so:

```javascript
var onlyUnique = function(string){
  var resultString = '';
  var storageObject = {};

  for(var i = 0; i < string.length; i++) {
    if(storageObject[string[i]]) {
      storageObject[string[i]]++
    } else {
      storageObject[string[i]] = 1; 
    }
  }

  for(var key in storageObject) {
    if(storageObject[key] === 1) {
      resultString = resultString + key;
    }
  } 
  return resultString;
};
```
So as you can see, we added the key to the resultString everytime the value was 1, and that will return all unique values of the input string.



Thanks for reading!




