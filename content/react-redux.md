# React + Redux


## Redux
##### redux-trunk
[Read this for an in-depth introduction to thunks in Redux.](http://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559)

Only plain object will go to store. A function will be called by redux-trunk and has no relationship with store itself except the dispatch argument.
Maybe we could consider it as a dispatch chain.
