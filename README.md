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
MomentJS |  meteor add momentjs:moment | Time format and conversion
Meteor Files | meteor add ostrio:files | File upload
Slingshot | meteor add edgee:slingshot | File upload (direct from client to S3 bypassing the meteor server
Autoforms | $ meteor add aldeed:autoform | create forms using schema
cfs | meteor add cfs:standard-packages | File upload to file system, mongo grid fs, etc.
cfs-ui | meteor add cfs:ui | progress bars, etc..
Spin | meteor add sacha:spin | Show a spinning wheel while waiting
Toastr | meteor add chrismbeckett:toastr | Show non-blocking notifications



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
