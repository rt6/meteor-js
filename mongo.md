# MONGO DB

### Add item to array field in a document
- `$push`, this will add item to the array. If the array does not exists, it will be created in the document
- `$addToSet`, this will add item to the array.  This will crash if the array is not already in the document

### Delete field from document 
```js
db.myCollection.update(
  {},
  { $unset: {'profile:friends':1}},
  false, true
);
```
