# meteor-js

Tips and Tricks

# Design Pattern

- assets/
  - img/
  - styles/
- client/
  - home/
    - home.html
    - homeLayout.html
    - homeHelper.js
  - feature2
  - feature3
- server/
  - feature2.js (containers server methods and publications)
  - feature3.js
- common/
  - collections.js
  - routes.js  

Notes:
- Client DDP subscriptions can be done in `routes.js` as part of the route configuration or in the helper.js file in the `client` directory.
