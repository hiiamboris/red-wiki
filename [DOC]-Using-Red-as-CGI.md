To use Red as CGI, you must configure your server to support Red executable. For Apache, see https://httpd.apache.org/docs/current/howto/cgi.html .

Basic example script would then look like this:
```red
#!/usr/local/bin/red ; replace this with your path to Red executable
Red[]
print "Content-type: text/html^/"
print rejoin ["Hello world from Red " system/version]
```


to print the HTTP header:
```
call/wait/output "printenv" o: "" print o
```

If you want to process data passed to CGI, I suggest using [HTTP tools](https://github.com/rebolek/red-tools/blob/master/http-tools.red) that will read HTTP headers and parse them to `http-headers` object. This object has same structure as Rebol's `system/options/cgi`, described [here](http://www.rebol.com/docs/cgi2.html#section-6).

For the `GET` method, query string is in `http-headers/query-string`.

For the `POST` method, you can get the POST data using `input` or `read-stdin`:

```red
#!/usr/local/bin/red
Red[]
print "Content-type: text/html^/"
print rejoin ["Post data: " mold input]
```

Please note that `input` is good for textual data only if you want to get binary data, you need to use `read-stdin` and pre-allocate binary buffer:

```red
#!/usr/local/bin/red
Red[]
length: 100'000
read-stdin data: make binary! length length
print "Content-type: text/html^/"
print rejoin ["Post data: " mold read-stdin]
```

Be sure that you allocate big enough buffer, as `read-stdin` won't auto expand the buffer.
## Under IIS 8.5

To use Red as CGI under IIS, follow the instructions below:

* Find the compiled console executable (mostly under `C:\ProgramData\Red`)
* Copy and rename the Red console application to somewhere that IIS can reach and execute
* Open Internet Information Services Manager, go to your site settings, select `Handler Mappings` and select `Add Module Mapping`
* Type `*.red` to `Request Path`, `<path>\redconsole.exe "%s"` to `Executable`, `Red` to `Name`
* Select `Yes` for `allow this ISAPI extension?` question
* Under `CGI`, set `Use New Console For Each Invocation` to `true`. Otherwise you get `502.2 - Bad Gateway. SetConsoleTitle failed!` error.

If you use `red.exe` instead of compiled red console application, then don't forget to put `--cli` for `Executable` settings: `<path>\red.exe --cli "%s"`. Otherwise you get `502.2 - Bad Gateway. ""` (empty header error)

## Under Cheyenne Web Server

For details see https://www.cheyenne-server.org/about.shtml

Add `.red` extension to `bind-extern` in `globals` section in `httpd.cfg` file:

```red
globals [
  listen [80 81 82] ;can listen multiple ports
  bind-extern CGI to [.cgi .red] ;you can use any extension you want
  bind-extern RSP to [.rsp]
  worker-libs [
    ...
```

Reload config or restart Cheyenne.

Then create a file named `test.red` under `www`, change the path to your `red` executable or the pre-compiled console executable in the first line of your script:

```red
#!/E/Server/cheyenne/www/red-console.exe
Red []
print "Content-type: text/html^/"
print now
```

Now you are ready to test. Open a browser and point to `localhost/test.red` you should see the output of your script.


## Lighttpd Web Server on OS X Mojave

To use Red as CGI under lighttpd, follow the instructions below:

Install HomeBrew by copying this line into the Terminal

```/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```

then install Lighttpd by typing this into the terminal:

```brew install lighttpd```

then open the following file:
```/usr/local/etc/lighttpd/lighttpd.conf```

and add ```index.red``` like you see below:

```
index-file.names += (
  "index.xhtml", "index.html", "index.htm", "default.htm", "index.red", "index.php"
)
```


then add the ```.red``` extension like you see below so red files can be executed:
```
static-file.exclude-extensions = ( ".php", ".pl", ".fcgi", ".scgi", ".red" )
```

then open the file located at ```/usr/local/etc/lighttpd/conf.d/cgi.conf```
and add ```.red``` like you see below:

```
cgi.assign                 = ( ".pl"  => "/usr/bin/perl",
                               ".cgi" => "/usr/bin/perl",
                               ".rb"  => "/usr/bin/ruby",
                               ".erb" => "/usr/bin/eruby",
                               ".red" => "/usr/local/bin/red.dms",
                               ".py"  => "/usr/bin/python" )
```

finally open this file:
```
/usr/local/etc/lighttpd/lighttpd/modules.conf
```

and un-comment this line:

```
include "conf.d/cgi.conf"
```

then start the server by typing this into the terminal:
```
lighttpd -f /usr/local/etc/lighttpd/lighttpd.conf
```

when you submit an HTML form POST in RED use the function `input` to read the text being sent

it will take some time the first time since RED will be compiling