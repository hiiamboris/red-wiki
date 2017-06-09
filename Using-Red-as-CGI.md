To use Red as CGI, you must configure your server to support Red executable. For Apache, see https://httpd.apache.org/docs/current/howto/cgi.html .

Basic example script would then look like this:
```
#!/usr/local/bin/red ; replace this with your path to Red executable
Red[]
print "Content-type: text/html^/"
print rejoin ["Hello world from Red " system/version]
```

If you want to process data passed to CGI, I suggest using [HTTP tools](https://github.com/rebolek/red-tools/blob/master/http-tools.red) that will read HTTP headers and parse them to `http-headers` object. This object has same structure as Rebol's `system/options/cgi`, described [here](http://www.rebol.com/docs/cgi2.html#section-6).

For the `GET` method, query string is in `http-headers/query-string`.

For the `POST` method, you can get the POST data using `input`:

```
#!/usr/local/bin/red
Red[]
print "Content-type: text/html^/"
print rejoin ["Post data: " mold input]
```
