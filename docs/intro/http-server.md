# Creating a simple HTTP application

Cro HTTP applications are hosted using its built-in HTTP server. To start
out, we import it:

```
use Cro::HTTP::Server;
```

The server hosts an applicatin, which is something that transforms a
`Cro::HTTP::Request` instance representing the request into a
`Cro::HTTP::Resposne` instance representing the response. While it's possible
to write that "by hand", it is far more convenient to use the HTTP router.

```
use Cro::HTTP::Router;
```

The `Cro::HTTP::Router` module provides a convenient API for mapping incoming
requests to appropriate handlers. It also provides routines to make it quick
and easy to produce the most common kinds of responses. Here is a simple
example:

```
my $application = route {
    get -> {
        content 'text/html', 'Hello World!';
    }
}
```

Routines like `get`, `post`, and `delete` indicate the HTTP method,
while the signature part represents the target of the request. The
absence of parameters counts as `/`. Let's see some more examples:

```
my $application = route {
    get -> {
        content 'text/html', 'Home';
    }
    post -> 'poster' {
        content 'text/html', 'Post request to /poster page';
    }
    get -> 'articles', $author, $name {
        # Returns an article for `/articles/anonymous/first_post` URL
    }
}
```

An application created with `route` can be easily used with
`Cro::HTTP::Server`:

```
# Creating a server...
my Cro::Service $service = Cro::HTTP::Server.new(
    :host('localhost'), :port(2314), :$application
);
# Running it
$service.start;
# Add up a react block to keep running until Ctrl-C signal
react whenever signal(SIGINT) {
    $hello-service.stop;
    exit;
}

```

And the application is up and running, so it's time to improve it: add
more logic and use other features. See `Cro::HTTP::Router` and
`Cro::HTTP::Server` to learn more.