# Ultra fast development

### Packages Required:
```sh
# install iron-cli
npm install -g iron-meteor

# create your app
iron create myapp01

# install autoform and collection2
iron add aldeed:autoform aldeed:collection2 twbs:bootstrap

# iron list
display installed packages

# don't forget to remove these packages
iron remove autopublish insecure
```

### 1. Create CRUD for new collection
``` sh
# create collection (this creates a file app/lib/collection/todo.js )
# open todo.js and update the allow and deny rules

iron g:collection todos
```

Add the form validation code for the schema to `todo.js` 
```js
Todos.attachSchema(new SimpleSchema({
  field: {
    type: String,
    label: "field label",
    max: 100
  },
  field2: {
    type: String,
    label: "field two",
    allowedValues: ['val1', 'val2', 'val3', 'val4'],
    optiona: true
  },
  field3: {
    type: Number,
    label: "field three",
    optional: true
  }
}));
```

Create publication so todo is accessible from client
```sh
iron g:publish todos
```

Create template for insert todo form
```sh
iron g:template todos/create_todo
```
This will create these 3 files in `app/client/template/todos/create_todo/`:
- create_todo.css
- create_todo.html
- create_todo.js

QuickForms will automatically generate insert form and do form validation (based on SimpleSchema).  Add a quickForm to `create_todo.html`:
```html
<template name="CreateTodo">
  <h1>Create New Todo</h1>
  {{> quickForm collection="Todos" id="insertTodoForm" type="insert" buttonContent="Create"}}
</template>
```

Create a controller to be used with iron-router.  This will create `todos_crontroller.js` in `app/lib/controllers`
```sh
iron g:controller Todos
```

Add insert method to `Todos_controller.js`:
```js
TodosController = RouteController.extend({
  subscriptions: function () {
    this.subscribe('todos');
  },
  data: function () {
  },
  create: function() {
    this.render('CreateTodo', {});
  }
});
```

Create a path to create todos.  Iron router will call the insert method of the TodosController:
```js
Router.route('/todos/create', {
  name: 'createTodos',
  controller: 'TodosController',
  action: 'create',
  where: 'client'
});
```

Start meteor server (this is equivalent to executing $meteor) in the /app directory
```sh
iron run

# goto /app directory
meteor mongo
```

