[![Go Report Card](https://goreportcard.com/badge/github.com/echocat/caddy-filter)](https://goreportcard.com/report/github.com/echocat/caddy-filter)

# caddy-filter

Provides a directive to filter response bodies. 

* [Usage](#usage)
* [Examples](#examples)
* [Contributing](#contributing)
* [License](#license)

## Usage

Add ``filter`` blocks to your CaddyFile.

```
filter {
    path <regexp pattern>
    content_type <regexp pattern> 
    search_pattern <regexp pattern>
    replacement <replacement pattern>
}
```

> **Important:** Define ``path`` and/or ``content_type`` not to open. Slack rules could dramatically impact the system performance because every response is recorded to memory before returning it.

* **``path``**: Regular expression that matches the requested path.
* **``content_type``**: Regular expression that matches the requested content type that results after the evaluation of the whole request.
* **``search_pattern``**: Regular expression to find in the response body to replace it.
* **``replacement``**: Pattern to replace the ``search_pattern`` with. 
    <br>You can use parameters. Each parameter must be formatted like: ``{name}``.
    * Regex group: Every group of the ``search_pattern`` could be addressed with ``{index}``. Example: ``"My name is (.*?) (.*?)." => "Your name is {2}, {1}."``
    * Request context: Parameters like URL ... could be accessed.
    <br>Example: ``Host: {request_host}``
        * ``request_header_<header name>``: Contains a header value of the request, if provided or empty.
        * ``request_url``: Full requested url
        * ``request_path``: Requested path
        * ``request_method``: Used method
        * ``request_host``: Target host
        * ``request_proto``: Used proto
        * ``request_proto``: Used proto
        * ``request_remoteAddress``: Remote address of the calling client
        * ``response_header_<header name>``: Contains a header value of the response, if provided or empty.

## Examples

Replace in every text file ``Foo`` with ``Bar``.

```
filter {
    path .*\.txt
    search_pattern "Foo"
    replacement "Bar"
}
```

Add Google Analytics to every HTML page.

```
filter {
    path .*\.html
    search_pattern "</title>"
    replacement "</title><script>(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)})(window,document,'script','//www.google-analytics.com/analytics.js','ga');ga('create', 'UA-12345678-9', 'auto');ga('send', 'pageview');</script>"
}
```

Insert server name in every HTML page

```
filter {
    content_type text/html.*
    search_pattern "Server"
    replacement "This site was provided by {response_header_Server}"
}
```

## Contributing

caddy-filter is an open source project by [echocat](https://echocat.org).
So if you want to make this project even better, you can contribute to this project on [Github](https://github.com/echocat/caddy-filter)
by [fork us](https://github.com/echocat/caddy-filter/fork).

If you commit code to this project, you have to accept that this code will be released under the [license](#license) of this project.

## License

See the [LICENSE](LICENSE) file.
