### Class properties

#### `add(model)`

Adds a model to a collection and is what [`save()`](#save) calls internally if it is successful. `add()` won't allow you to add a model to the collection if one already exists with the same [`id`](#id) or [`uid`](#uid).

    Food.all()
    // => []

    var egg = new Food({ id: 1, name: "egg" })
    var ham = new Food({ id: 2, name: "ham" })
    var cheese = new Food({ id: 3, name: "cheese" })

    Food.add(egg)
    Food.all()
    // => [egg]

    Food.add(ham, cheese)
    Food.all()
    // => [egg, ham, cheese]

    var not_egg = new Food({ id: 1, name: "not egg" })

    Food.add(not_egg)
    Food.all()
    // => [egg, ham, cheese]

#### `all()`

Returns an array of the models contained in the collection.

    Food.all()
    // => [egg, ham, cheese]

    Food.select(function() {
      return this.attr("name").indexOf("e") > -1
    }).all()
    // => [egg, cheese]

#### `chain(arrayOfModels)`

A utility method to enable chaining methods on a collection -- used internally by [`select()`](#select) for instance.

#### `count()`

Returns the size of the collection.

    Food.count()
    // => 3

    Food.select(function() {
      return this.attr("name").indexOf("e") > -1
    }).count()
    // => 2

#### `detect(func)`

Operates on the collection returning the first model that matches the supplied function.

    Food.detect(function() {
      return this.attr("name") == "ham"
    })
    // => ham

#### `each(func)`

Iterates over the collection calling the supplied function for each model.

    Food.each(function() {
      console.log(this.attr("name"))
    })
    // => logs "egg", "ham" and "cheese"

    Food.select(function() {
      return this.attr("name").indexOf("e") > -1
    }).each(function() {
      console.log(this.attr("name"))
    })
    // => logs "egg" and "cheese"

#### `find(id)`

Returns the model with the corresponding id.

    Food.find(2)
    // => ham

    Food.find(69)
    // => undefined

#### `first()`

Returns the first model in the collection.

    Food.first()
    // => egg

    Food.select(function() {
      return this.attr("name").indexOf("h") > -1
    }).first()
    // => ham

#### `last()`

Returns the last model in the collection.

    Food.last()
    // => cheese

#### `load(callback)`

Calls [`read()`](#read) on the persistence adapter and adds the returned models to the collection. The supplied callback is then passed an array of the newly added models.

    Food.load(function(models) {
      // do something...
    })

#### `map(func)`

Operates on the collection returning an array of values by calling the specified method on each instance.

    Food.map(function() {
      return this.attr("name").toUpperCase()
    })
    // => ["EGG", "HAM", "CHEESE"]

    Food.select(function() {
      return this.attr("name").indexOf("e") > -1
    }).map(function() {
      return this.attr("name").toUpperCase()
    })
    // => ["EGG", "CHEESE"]

#### `new(attributes)`

Instantiates a model, the supplied attributes get assigned directly to the model's [`attributes`](#attributes). Custom initialization behaviour can be added by defining an [`initialize()`](#initialize) instance method.

    var fish = new Food({ name: "fish" })

    fish.attributes
    // => { name: "fish" }

    fish.changes
    // => {}

#### `pluck(attributeName)`

Operates on the collection returning an array of values for the specified attribute.

    Food.pluck("name")
    // => ["egg", "ham", "cheese"]

    Food.select(function() {
      return this.attr("name").indexOf("e") > -1
    }).pluck("name")
    // => ["egg", "cheese"]

#### `remove(model)`

Removes a model from a collection.

    Food.all()
    // => [egg, ham, cheese]

    Food.remove(egg)
    Food.all()
    // => [ham, cheese]

#### `reverse()`

Returns a collection containing the models in reverse order.

    Food.reverse().all()
    // => [cheese, ham, egg]

#### `select(func)`

Operates on the collection returning a collection containing all models that match the supplied function.

    Food.select(function() {
      return this.attr("name").indexOf("e") > -1
    }).all()
    // => [egg, cheese]

#### `sort(func)`

Acts like `Array#sort()` on the collection. It's more likely you'll want to use [`sortBy()`](#sortby) which is a far more convenient wrapper to `sort()`.

#### `sortBy(attributeName` or `func)`

Returns the collection sorted by either an attribute or a custom function.

    Food.sortBy("name").all()
    // => [cheese, egg, ham]

    Food.sortBy(function() {
      return this.attr("name").length
    }).all()
    // => [egg, ham, cheese]
