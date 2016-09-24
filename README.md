# meteor-js

Tips and Tricks

# Design Pattern

- public/
  - img/
- client/
  - home/
    - home.html
    - homeLayout.html
    - homeHelper.js
  - feature2
  - feature3
  - lib/semantic-ui
- server/
  - methods.js
  - accessRules.js
  - publications.js
- common/
  - collections.js
  - routes.js  

Notes:
- Client DDP subscriptions can be done in `routes.js` as part of the route configuration or in the `helper.js` file in the `client` directory.

# Useful Packages
Package Name| Installation | Why use it
--- | --- | ---
Iron Router | meteor add iron:router | App Navigation
Semantic-UI | meteor add semantic:ui | All components and themes
accounts |  meteor add accounts-password | Password support for accounts
MomentJS |  meteor add momentjs:moment | Time format and conversion
Meteor Files | meteor add ostrio:files | File upload
Slingshot | meteor add edgee:slingshot | File upload (direct from client to S3 bypassing the meteor server
Autoforms | $ meteor add aldeed:autoform | create forms using schema
cfs | meteor add cfs:standard-packages | File upload to file system, mongo grid fs, etc.
cfs-ui | meteor add cfs:ui | progress bars, etc..
Spin | meteor add sacha:spin | Show a spinning wheel while waiting
Toastr | meteor add chrismbeckett:toastr | Show non-blocking notifications
Reactive Var | meteor add reactive-var | Make your variables reactive with Tracker



# How to setup Semantic-ui
```sh
# install 
$ meteor add semantic:ui

# create a blank file
$ touch lib/semantic-ui/custom.semantic.json

# run meteor to generate default configs by semantic-ui package 
$ meteor

# remove (note the '.' at start of filename
$ rm lib/semantic-ui/.custom.semantic.json

# run meter to download semantic files
$ meteor

# now you can start using semantic-ui in your code!
```


### Query MongoDB using criteria and return selected fields

```json
// In this example, return image objects that match an owner id.  Return the image _id, item name, owner _id, owner name.
db.cfs.images.filerecord.find(
  {"metadata.owner_id":"abcdefg123"},
  {"metadata.itemName":1, "metadata.owner_firstName":1}
  ).pretty();
  
// return the user id and their profile first name, last name and role
db.users.find(
  {},
  {"_id":1, "profile":1}
).pretty();
```

### Add more values to exisiting MongoDB document
```json
# add an item to array called "toys"
# note push will add the item to the array even if it already exists
# if you do not want duplicates, use $addToSet
db.collectionName.update(
  {"owner_id":"jadkfjkladflkadflkj"}, 
  {$push: {
      "Toys": { "name":"bike","location":"garage" } 
    }
  }
)

# to delete elements from array use $pop, $pull, $pullAll
... example coming soon..

```

### Access the logged in user's details
In **client-side javascript** helpers:
```javascript
// use these methods from accounts package
Meteor.userId();
Meteor.user()._id;
Meteor.user().profile.firstName; 
```

in **blaze** views:
```html
<table class="ui definition table">
{{#with currentUser}}
    <tr>
        <td>User ID:</td>
        <td>
            {{_id}}
        </td>
    </tr>
        {{#with profile}}
    <tr>
        <td>First Name: </td>
        <td>
        {{firstName}}
        </td>
    </tr>
    <tr>
        <td>Last Name:</td>
        <td>
            {{lastName}}
        </td>
    </tr>
    {{/with}}
{{/with}}
</table>
```

in **server-side publications** (you must use this.userId):
```js
Meteor.publish("Items", function(){
  return Items.find({"owner_id": this.userId});
  
  // return itemName and description fields only (this will reduce server load, bandwidth and make the page load faster)
  return Items.find({"owner_id": this.userId},{itemName:1, description:1});
}
```

### Helper functions that are accessible by all templates
Meteor loads the files in the deepest nested directory **first**.  Sometimes it is hard to predict if your helper functions will be loaded or not, and you do not want to write the same code for helper functions (eg. formatDate) in each Template.
```js
Template.registerHelper(
  'formatDate', function (d){ 
    var m = moment(d);
    return m.format("DD MMM YYYY HH:mm");
  }
);
```

### Iron Router

```js
// configure 404 File Not Found and page loading template
Router.configure({
    notFoundTemplate: 'notFound404',
    loadingTemplate: 'loading'
});

//redirect user if they are not logged in
Router.onBeforeAction( function() {
    if (!Meteor.userId() && !Meteor.loggingIn()){
        //this.redirect('login');
        this.redirect('home');
        toastr.error('Please login in first');
        this.stop();
    }else{
        this.next();
    }
}, {except:['login','home']});


Router.route('/showItem/:_id', {
    name: "showItem",
    template: "showItem",
    layoutTemplate: "siteLayout",
    //continue showing loading page until Images subscription is complete
    waitOn: function() {
        Meteor.subscribe("Images");
    },
    //return an Image object to the template to render
    data: function(){
        return Images.findOne({_id: this.params._id});
    }
});

```
