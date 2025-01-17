# Sorting

Sorting is a [GORM](https://github.com/jinzhu/gorm) plugin, it adds sorting and reordering abilities to models.

It will add a position column into the model using sorting, and automatically increase its value after create new record. When some records deleted it will reorder all positions, to make sure there is no gap in the sequence postions

## Usage

Sorting using GORM callbacks to handle sorting and reordering, so you need to register callbacks to gorm DB first, like:

```go
import (
  "github.com/jinzhu/gorm"
  "github.com/qor/sorting"
)

func main() {
  db, err := gorm.Open("sqlite3", "demo_db")
  sorting.RegisterCallbacks(&db)
}
```

### Sort Modes

Sorting has defined two modes which could be used as anonymous field in model.

- Ascending mode (smallest first)
- Descending mode (smallest last)

It could be used like:
```go
// Ascending mode
type Category struct {
  gorm.Model
  sorting.Sorting // this will register a `position` column to model Category, add it with gorm AutoMigrate
}

db.Find(&categories)
// SELECT * FROM categories ORDER BY position;

// Descending mode
type Product struct {
  gorm.Model
  sorting.SortingDESC // this will register a `position` column to model Product, add it with gorm AutoMigrate
}

db.Find(&products)
// SELECT * FROM products ORDER BY position DESC;
```

### Reorder

```go
// Move Up
sorting.MoveUp(&db, &product, 1)
// If a record is in positon 5, it will be brought to 4

// Move Down
sorting.MoveDown(&db, &product, 1)
// If a record is in positon 5, it will be brought to 6

// Move To
sorting.MoveTo(&db, &product, 1)
// If a record is in positon 5, it will be brought to 1
```

## Qor Support

[QOR](http://getqor.com) is architected from the ground up to accelerate development and deployment of Content Management Systems, E-commerce Systems, and Business Applications, and comprised of modules that abstract common features for such system.

Although Sorting could be used alone, it works nicely with QOR, if you have requirements to manage your application's data, be sure to check QOR out!

[QOR Demo:  http://demo.getqor.com/admin](http://demo.getqor.com/admin)

[Sorting Demo with QOR](http://demo.getqor.com/admin/colors?sorting=true)

## License

Released under the [MIT License](http://opensource.org/licenses/MIT).
