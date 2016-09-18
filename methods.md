# Server Methods

In production you do not want to give all clients access to the entire MongoDB.  
Server methods can be useful to restrict access to MongoDB collections, and/or do server-side processing.

### Examples functions that can use methods:
- Find friends
- Add friends
- Hold seat 
- Book ticket

### Server-side method
In the `server` directory, create a file called `methods.js`:
```js
Meteor.methods({
  'server_method_name': function( arg1 ){
    if (arg1 == null || arg1 ===''){
        console.log('no search parameters provided');
        return [];
    } else {
        var query = RegExp (arg1, 'i');
        
        // mongodb find() returns a cursor. Use fetch() to retrieve actual documents
        var r = Meteor.users.find({"profile.firstName": query},
            {fields:{"profile.firstName":1, "profile.lastName":1, _id:1}}).fetch();

        return r;
    }
  }  
});
```

### Client-side invoke method
Invoke server method and pass the arg1.  This is an async call. 
When mongodb replies with results (the callback is fired), then write the returned results in a session variable.  
Do not try to return mongodb results as part of the template handler code as it will not work.  
```js
// inside template event handler
Meteor.call('server_method_name', arg1, function(error, result){
  if (error){
    alert(error);
  } else {
    Session.set("results", result);
  }
}
```

