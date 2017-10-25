# fn-plugin-example

Example for adding third party plugins to fn project server.

This repo includes an example called `logspam`.

## Method 1 - Go style

This method is good for extension developers as it's fast builds for dev/testing.

Copy [main.go](https://github.com/fnproject/fn/blob/master/main.go) from `fn` repo, then add plugins. See [main.go](main.go) in this 
repo for an example.

Then assuming you have the fn project in your GOPATH or you've vendored it here, it should build:

```sh
go build -o fn-server
./fn-server
```

Then deploy a function and you'll see the special spam output like this:

```
INFO[0000] Serving Functions API on address `:8080`
YO! This is an annoying message that will happen every time a function is called.
YO! And this is an annoying message that will happen AFTER every time a function is called.
```

## Method 2 - Docker style

This method is good for end users that just want to add already developed/tested extensions. It's easier, but slower.

First copy main.go and add your extensions, see [main.go](main.go) for an example.

Then make a Dockerfile with only this in it:

```
FROM fnproject/functions
```

Then run:

```sh
docker build -t treeder/fn-custom .
```

Replace `treeder/fn-custom` with what you want the image to be called.

Then start it with:

```sh
docker run --rm -it --name functions -v /var/run/docker.sock:/var/run/docker.sock -v $PWD/data:/app/data -p 8080:8080 treeder/fn-custom
```

Now if you deploy a function and call it:

```sh
fn init --runtime go myfunc
cd myfunc
fn deploy --local --app myapp
fn call myapp /myfunc
```

You'll see our annoying output that this extension adds!
