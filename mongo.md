# MONGO DB

### Add item to array field in a document
- `$push`, this will add item to the array. If the array does not exists, it will be created in the document
- `$addToSet`, this will add item to the array.  This will crash if the array is not already in the document

### Delete field from document 
```js
// delete from all documents
db.myCollection.update(
  {},
  { $unset: {'profile:friends':1}},
  false, true
);

// delete from document matching _id using field selector
db.myCollection.update(
  {_id: 1234},
  { $unset: {'profile:friends':1}},
  false, true
);


// for document _id abcde123, delete array element if its _id matches "tobedelete_id"
db.users.update(
  {_id: "abcde123"},
  {$pull: {'profile.friends':{_id:"tobedelete_id"}}}
);
```

## find, findOne, fetch
- `find()` return cursor 
- `fetch()` return array of documents
- `findOne()` return one document

operators
$nin, $ne, $gt, $lt
