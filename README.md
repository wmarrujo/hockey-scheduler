# hockey-scheduler
a small scheduling optimizer for hockey games

## Usage

input a `toml` file into the command line application as the first argument

## Building

```
$ ./make.sh
```

This will build the python to the build directory

## Testing

```
$ ./make.sh && python3 build test.toml output.csv
```

This will build & run the program with an input of `test/test.toml`