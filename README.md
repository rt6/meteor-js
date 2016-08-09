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


### Access the logged in user's details
In **javascript** helpers:
```javascript
Meteor.user().profile._id;
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
