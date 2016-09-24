# SECURITY

### check user input
Check that inputs are strings incase they are mongo selectors (ie. contain "{}")

First install the `check` package
```sh
$ meteor add check
```

Then in all client and espcially server side metthods and publications check user inputs:

```js
Meteor.methods({
  'myMethod': function(userId){
    check(userId, String);
  }
});
```
