### Sentinel errors

- Global variables that wrap an error object

- Created using errors.New("string")

##### errors.Is(passedinError, matchingError error) bool

- checks the error and any error it wraps if it matches target

##### errors.As(err error, target interface{} ) bool

- returns the target error matching the passed in error
