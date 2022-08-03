# Mongo Db

## Create DB

```
use databaseName
```

## To see what is there in db

```
show collections
```

## Insert into dogs collection

```
db.dog.insertOne({name:"paul",age:12})
```

```
db.dogs.insert([{name:"wyat",age:11},{name:"tanya",age:3}])
```

## To see data base

```
db.dog.find()
```

## To search in a database

```mongodb
 db.dogs.findOne({age:3})//fings one dog with age 3
```

```
 db.dogs.find({age:3})//finds all dogs with age 3
```

## Updation

```
db.dog.updateOne({name:'pauil'},{$set:{age:4}})
```

` first part`: condition \
 `$set` : this updates everything within {}\
 `updateMany()` \
`$currentDate`

```
  db.dog.updateOne({name:'pauil'},{$set:{age:4}},{$currentDate:{lastmod:true}})
```

`result`

```
 db.dog.findOne()
{
        "_id" : ObjectId("6088523ccfba2585ac518970"),
        "name" : "paul",
        "age" : 23,
        "lastmod" : ISODate("2021-04-27T18:39:45.455Z")
}
```

---

## Deletion

```
db.dog.deleteOne({name:'paul'})   => deletes where name is paul
```

```
db.dog.deleteMany({}) =>deletes all
```

---

## Operators

`db.inventory.find( { qty: { $eq: 20 } } ) ` - equal to \
similar

- `$gt`
- `$gte`
- `$lte`
- `$ne`

`db.inventory.find( { qty: { $in: [ 5, 15 ] } } )` - qty is either 5 or 15\
simliler

- `$nin`

---

## mongoose

```javascript
const mongoose = require("mongoose");
mongoose
  .connect("mongodb://localhost:27017/movieApp", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => {
    console.log("CONNECTION OPEN!!!");
  })
  .catch((err) => {
    console.log("OH NO ERROR!!!!");
    console.log(err);
  });

const movieSchema = new mongoose.Schema({
  title: String,
  year: Number,
  score: Number,
  rating: String,
});

const Movie = mongoose.model("Movie", movieSchema);
// const amadeus = new Movie({ title: 'Amadeus', year: 1986, score: 9.2, rating: 'R' });

// Movie.insertMany([               //returns promise
//     { title: 'Amelie', year: 2001, score: 8.3, rating: 'R' },
//     { title: 'Alien', year: 1979, score: 8.1, rating: 'R' },
//     { title: 'The Iron Giant', year: 1999, score: 7.5, rating: 'PG' },
//     { title: 'Stand By Me', year: 1986, score: 8.6, rating: 'R' },
//     { title: 'Moonrise Kingdom', year: 2012, score: 7.3, rating: 'PG-13' }
// ])
//     .then(data => {
//         console.log("IT WORKED!")
//         console.log(data);
//     })
```

---

## finding in mongoose

```javascript
Movie.find({}).then((data) => console.log(data)); //same as mongo query this is a thenable abject but not a promise
Movie.findOne({});
Movie.findById;
```

---

## updating in mongoose

```javascript
Movie.updateOne({ title: "amadeus" }, { year: 1985 });
Movie.updateMany({});
Movie.findOneAndUpdate({}, {}, { new: true }).then((m) => console.log(m)); //gives the new updated value
```

---

## deleting in mongoose

```javascript
Movie.remove({ query });
Movie.deleteMany({ query });
Movie.findOneAndDelete({}).then((m) => console.log()); //to get the delete data this method gives back the information on update
```

---

## validators and validation errors

```javascript
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true, //error if not present when created
    maxlength: 20, //cannot exceed 20
  },
  price: {
    type: Number,
    required: true,
    min: [0, "Price must be positive ya dodo!"], //validation error message if less than 0
  },
  onSale: {
    type: Boolean,
    default: false,
  },
  categories: [String], //stores a array of strings
  qty: {
    online: {
      type: Number,
      default: 0, //default
    },
    inStore: {
      type: Number,
      default: 0,
    },
  },
  size: {
    type: String,
    enum: ["S", "M", "L"], //cannot be any other character
  },
});
const bike = new Product({
  name: "Cycling Jersey",
  price: 28.5,
  categories: ["Cycling"],
  size: "XS",
}); // it will give error as size not in list given
```

---

# Instance Methods

```javascript
productSchema.methods.greet = function () {
  console.log("HELLLO!!! HI!! HOWDY!!! ");
  console.log(`- from ${this.name}`);
};

productSchema.methods.toggleOnSale = function () {
  this.onSale = !this.onSale;
  return this.save();
};

productSchema.methods.addCategory = function (newCat) {
  this.categories.push(newCat);
  return this.save(); //after updating save
};
```

- while using this use await while calling function as save takes some time
- Instance methods deal with that instance of the model
- `this` - that product which we ar using the method
- `format is`

```javascript
schemaObject.methods.funcName = function () {
  //dont use ()=> here you cant use `this` then
};
```

---

## static methods

```javascript
productSchema.statics.fireSale = function () {
  return this.updateMany({}, { onSale: true, price: 0 });
};
```

- here the method works on the whole model not an instance so all instances of a model will get changed

---

## example

```javascript
const mongoose = require("mongoose");
mongoose
  .connect("mongodb://localhost:27017/shopApp", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => {
    console.log("CONNECTION OPEN!!!");
  })
  .catch((err) => {
    console.log("OH NO ERROR!!!!");
    console.log(err);
  });

const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    maxlength: 20,
  },
  price: {
    type: Number,
    required: true,
    min: [0, "Price must be positive ya dodo!"],
  },
  onSale: {
    type: Boolean,
    default: false,
  },
  categories: [String],
  qty: {
    online: {
      type: Number,
      default: 0,
    },
    inStore: {
      type: Number,
      default: 0,
    },
  },
  size: {
    type: String,
    enum: ["S", "M", "L"],
  },
});

productSchema.methods.greet = function () {
  console.log("HELLLO!!! HI!! HOWDY!!! ");
  console.log(`- from ${this.name}`);
};

productSchema.methods.toggleOnSale = function () {
  this.onSale = !this.onSale;
  return this.save();
};

productSchema.methods.addCategory = function (newCat) {
  this.categories.push(newCat);
  return this.save();
};

productSchema.statics.fireSale = function () {
  return this.updateMany({}, { onSale: true, price: 0 });
};

const Product = mongoose.model("Product", productSchema);

const findProduct = async () => {
  const foundProduct = await Product.findOne({ name: "Mountain Bike" });
  console.log(foundProduct);
  await foundProduct.toggleOnSale();
  console.log(foundProduct);
  await foundProduct.addCategory("Outdoors");
  console.log(foundProduct);
};

// Product.fireSale().then(res => console.log(res))

// findProduct();
const bike = new Product({
  name: "Cycling Jersey",
  price: 28.5,
  categories: ["Cycling"],
  size: "XS",
});
bike
  .save()
  .then((data) => {
    console.log("IT WORKED!");
    console.log(data);
  })
  .catch((err) => {
    console.log("OH NO ERROR!");
    console.log(err);
  });

Product.findOneAndUpdate(
  { name: "Tire Pump" },
  { price: 9 },
  { new: true, runValidators: true }
)
  .then((data) => {
    console.log("IT WORKED!");
    console.log(data);
  })
  .catch((err) => {
    console.log("OH NO ERROR!");
    console.log(err);
  });
```

---

## mongoose middleware

```javascript
personSchema.pre("save", async function () {
  //handles data before promise resolved
  this.first = "YO";
  this.last = "MAMA";
  console.log("ABOUT TO SAVE!!!!");
});
personSchema.post("save", async function () {
  //after a promise is resolved
  console.log("JUST SAVED!!!!");
});
```

---

## example

```javascript
const mongoose = require("mongoose");
mongoose
  .connect("mongodb://localhost:27017/shopApp", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => {
    console.log("CONNECTION OPEN!!!");
  })
  .catch((err) => {
    console.log("OH NO ERROR!!!!");
    console.log(err);
  });

const personSchema = new mongoose.Schema({
  first: String,
  last: String,
});

personSchema.virtual("fullName").get(function () {
  //virtual function
  return `${this.first} ${this.last}`;
});

personSchema.pre("save", async function () {
  this.first = "YO";
  this.last = "MAMA";
  console.log("ABOUT TO SAVE!!!!");
});
personSchema.post("save", async function () {
  console.log("JUST SAVED!!!!");
});

const Person = mongoose.model("Person", personSchema);
```

---

## EXample for rest using mongoose

`app.js`

```javascript
const express = require("express");
const app = express();
const path = require("path");
const mongoose = require("mongoose");
const methodOverride = require("method-override");
const Product = require("./models/products"); //import schema from mongoose from products.js
const { findByIdAndDelete } = require("./models/products");
mongoose
  .connect("mongodb://localhost:27017/FarmStand", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => {
    console.log("CONNECTION OPEN!!!");
  })
  .catch((err) => {
    console.log("OH NO ERROR!!!!");
    console.log(err);
  });
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");
app.use(express.urlencoded({ extended: true }));
app.use(methodOverride("_method"));
app.get("/products", async (req, res) => {
  const prod = await Product.find({});
  console.log(prod);
  res.render("products/index", { prod });
});
app.get("/products/new", (req, res) => {
  res.render("products/new");
});
app.post("/products", async (req, res) => {
  const { name, price, category } = req.body;
  const newEntry = new Product({
    name: name,
    price: price,
    category: category,
  });
  await newEntry.save();
  res.redirect("/products");
});
app.put("/products/:id/edit", async (req, res) => {
  const { id } = req.params;
  await Product.findByIdAndUpdate(id, req.body);
  res.redirect("/products");
});
app.get("/products/:id/edit", async (req, res) => {
  const { id } = req.params;
  const foundProduct = await Product.findById(id);
  res.render("products/edit", { foundProduct });
});
app.get("/products/:id", async (req, res) => {
  const { id } = req.params;
  const foundProduct = await Product.findById(id);
  console.log(foundProduct);
  res.render("products/show", { foundProduct });
});
app.delete("/products/:id", async (req, res) => {
  const { id } = req.params;
  await Product.findByIdAndDelete(id);
  res.redirect("/products");
});
app.listen(3000, () => {
  console.log("app listening on port 3000");
});
```

---

`products.js`

```javascript
const mongoose = require("mongoose");
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  price: {
    type: Number,
    required: true,
    min: 0,
  },
  category: {
    type: String,
    enum: ["fruit", "vegetable", "dairy"],
  },
});
const Product = mongoose.model("Product", productSchema);
module.exports = Product;
```

---

dealing with errors in mongoose

```javascript
const handleValidationErr = (err) => {
  console.dir(err);
  //In a real app, we would do a lot more here...
  return new AppError(`Validation Failed...${err.message}`, 400);
};

app.use((err, req, res, next) => {
  console.log(err.name);
  //We can single out particular types of Mongoose Errors:
  if (err.name === "ValidationError") err = handleValidationErr(err);
  next(err);
});
```

---

one to few

```javascript
const mongoose = require("mongoose");

mongoose
  .connect("mongodb://localhost:27017/relationshipDemo", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => {
    console.log("MONGO CONNECTION OPEN!!!");
  })
  .catch((err) => {
    console.log("OH NO MONGO CONNECTION ERROR!!!!");
    console.log(err);
  });

const userSchema = new mongoose.Schema({
  first: String,
  last: String,
  addresses: [
    {
      _id: { id: false },
      street: String,
      city: String,
      state: String,
      country: String,
    },
  ],
});

const User = mongoose.model("User", userSchema);

const makeUser = async () => {
  const u = new User({
    first: "Harry",
    last: "Potter",
  });
  u.addresses.push({
    street: "123 Sesame St.",
    city: "New York",
    state: "NY",
    country: "USA",
  });
  const res = await u.save();
  console.log(res);
};

const addAddress = async (id) => {
  const user = await User.findById(id);
  user.addresses.push({
    street: "99 3rd St.",
    city: "New York",
    state: "NY",
    country: "USA",
  });
  const res = await user.save();
  console.log(res);
};
```

# one to many - parent has link to child

```javascript
const productSchema = new Schema({
  name: String,
  price: Number,
  season: {
    type: String,
    enum: ["Spring", "Summer", "Fall", "Winter"],
  },
});

const farmSchema = new Schema({
  name: String,
  city: String,
  products: [{ type: Schema.Types.ObjectId, ref: "Product" }],
});

const Product = mongoose.model("Product", productSchema);
const Farm = mongoose.model("Farm", farmSchema);

// Product.insertMany([
//     { name: 'Goddess Melon', price: 4.99, season: 'Summer' },
//     { name: 'Sugar Baby Watermelon', price: 4.99, season: 'Summer' },
//     { name: 'Asparagus', price: 3.99, season: 'Spring' },
// ])

const makeFarm = async () => {
  const farm = new Farm({ name: "Full Belly Farms", city: "Guinda, CA" });
  const melon = await Product.findOne({ name: "Goddess Melon" });
  farm.products.push(melon);
  await farm.save();
  console.log(farm);
};

const addProduct = async () => {
  const farm = await Farm.findOne({ name: "Full Belly Farms" });
  const watermelon = await Product.findOne({ name: "Sugar Baby Watermelon" });
  farm.products.push(watermelon);
  await farm.save();
  console.log(farm);
};

Farm.findOne({ name: "Full Belly Farms" }) //to make objectid to ele
  .populate("products")
  .then((farm) => console.log(farm));
```

## one to bijjilion - child has link to parent

```javascript
const userSchema = new Schema({
  username: String,
  age: Number,
});

const tweetSchema = new Schema({
  text: String,
  likes: Number,
  user: { type: Schema.Types.ObjectId, ref: "User" },
});

const User = mongoose.model("User", userSchema);
const Tweet = mongoose.model("Tweet", tweetSchema);

// const makeTweets = async () => {
//     // const user = new User({ username: 'chickenfan99', age: 61 });
//     const user = await User.findOne({ username: 'chickenfan99' })
//     const tweet2 = new Tweet({ text: 'bock bock bock my chickens make noises', likes: 1239 });
//     tweet2.user = user;
//     tweet2.save();
// }

// makeTweets();

const findTweet = async () => {
  const t = await Tweet.find({}).populate("user");
  console.log(t);
};
```

---

## seting up 2 way relation

farm.js

```javascript
const farmSchema = new Schema({
  name: {
    type: String,
    required: [true, "Farm must have a name!"],
  },
  city: {
    type: String,
  },
  email: {
    type: String,
    required: [true, "Email required"],
  },
  products: [
    {
      type: Schema.Types.ObjectId,
      ref: "Product",
    },
  ],
});
const Farm = mongoose.model("Farm", farmSchema);
module.exports = Farm;
```

products.js

```js
const productSchema = new Schema({
  name: {
    type: String,
    required: true,
  },
  price: {
    type: Number,
    required: true,
    min: 0,
  },
  category: {
    type: String,
    lowercase: true,
    enum: ["fruit", "vegetable", "dairy"],
  },
  farm: {
    type: Schema.Types.ObjectId,
    ref: "Farm",
  },
});
const Product = mongoose.model("Product", productSchema);

module.exports = Product;
```

---

`interconnecting farm and products by adding product to farm and connecting product ack to farm`

```js
app.post("/farms/:id/products", async (req, res) => {
  const { id } = req.params;
  const farm = await Farm.findById(id);
  const { name, price, category } = req.body;
  const product = new Product({ name, price, category });
  farm.products.push(product);
  product.farm = farm;
  await farm.save();
  await product.save();
  res.redirect(`/farms/${id}`);
});
```

---

delete the products when a farm gets deleted

```js
// DELETE ALL ASSOCIATED PRODUCTS AFTER A FARM IS DELETED
farmSchema.post("findOneAndDelete", async function (farm) {
  //farm-deleted product - we us post middleware
  if (farm.products.length) {
    const res = await Product.deleteMany({ _id: { $in: farm.products } }); //deletes all products with id in farms.product
    console.log(res);
  }
});
```

---

deleteing review from a campground

```js
app.delete(
  "/campgrounds/:id/reviews/:reviewId",
  catchAsync(async (req, res) => {
    const { id, reviewId } = req.params;
    await campground.findByIdAndUpdate(id, { $pull: { reviews: reviewId } }); //updating the campground with id by deleting/pulling review with reviewId from the review of campground id
    await Review.findByIdAndDelete(reviewId);
    res.redirect(`/campgrounds/${id}`);
  })
);
```

---

when deleting a campground

```js
CampgroundSchema.post("findOneAndDelete", async function (doc) {
  if (doc) {
    await Review.deleteMany({
      _id: {
        $in: doc.reviews,
      },
    });
  }
});
```

---

## Virtuals in JSON

> By default, Mongoose does not include virtuals when you convert a document to JSON. For example, if you pass a document to Express' res.json() function, virtuals will not be included by default.

To include virtuals in res.json(), you need to set the toJSON schema option to { virtuals: true }.

```js
const opts = { toJSON: { virtuals: true } }; /////////////////
const userSchema = mongoose.Schema(
  {
    _id: Number,
    email: String,
  },
  opts
); ///
// Create a virtual property `domain` that's computed from `email`.
userSchema.virtual("domain").get(function () {
  return this.email.slice(this.email.indexOf("@") + 1);
});
```
---
# database injections
- download the below 
```
npm install express-mongo-sanitize
```
```js
const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());
```