---
layout: post
title:      "Fetching with Redux"
date:       2021-02-17 19:40:21 +0000
permalink:  fetching_with_redux
---


Redux and Fetch are both incredibly powerful tools for programmers. Redux allows our React App to separate responsibility, freeing us from keeping track of a complicated web of props passed all across the application by placing state all in one place, and giving each component access as needed. Fetch, for its part, allows our React (or broadly any JavaScript) app to communicate with a separate API. In this way, the API can persist data, leaving the frontend to worry exclusively about presenting data to the user.

Redux uses Actions, sent using the `dispatch` function, to tell the `reducer` what changed need to be made. In the case of major changes to user data, such as a username for a session or items added to a shopper's cart, the Redux global state (the store) and the API backend need to be in sync, which means `fetch` and a dispatched action need to happen together.

Since it is the backend's responsibility to actually handle data, the ideal flow is for a `fetch` call to be made to the backend, and for the resulting JSON to be sent to the store via a `dispatch` action. The problem is that fetch is asynchronous, so by calling fetch then making an action out of the result

``` 
const fetchSomething = () => {
   const something = fetch(somethingURL)
	    .then(resp => resp.json())
   return {
	    type: 'DO_SOMETHING',
			something
	 }
}
```

by the time the action is created, `something` is still a Promise Object and is not useful to the `reducer`.

## Enter Thunk

Thunk is middleware which allows us to return a function. If we pass `dispatch` to that function as an argument, it means our `fetchSomething` function can call dispatch our action *within* the fetch request, allowing us to take advantage of `.then()` to wait for the Promise Object to resolve before dispatching our action.

```
const fetchSomething = () => {
   return = (dispatch) => {
```
we pass `dispatch` as an argument so we can use it within the `return`
```
	    dispatch({type: "LOADING_SOMETHING})
```
we dispatch a loading action so the reducer knows data is coming
```
			fetch(somethingURL)
			.then(resp => resp.json())
			.then(somethingData => dispatch({type: "SOMETHING_LOADED", payload: somethingDate})
```
finally, after the Promise Object has been resolved into JSON, we can call `dispatch`, create an action, and send that JSON as the payload to the `reducer` to update the store.

Thunk is what allows us to pass `dispatch` to our `return`, and thereby tell it to wait until the Promise Object is resolved before dispatching our action. It is how Redux, an otherwise synchronous framework, handles asynch calls, like `fetch`.
	    

