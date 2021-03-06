# urlobject.py v0.5

`URLObject` is a utility class for manipulating URLs.

## Example Usage

Here is how you use the library:
    
    >>> from urlobject import URLObject
    >>> url = URLObject(scheme='http', host='example.com')
    >>> print url
    http://example.com/
    >>> print url / 'some' / 'path'
    http://example.com/some/path
    >>> print url & ('key', 'value')
    http://example.com/?key=value
    >>> print url & ('key', 'value') & ('key2', 'value2')
    http://example.com/?key=value&key2=value2
    >>> print url * 'fragment'
    http://example.com/#fragment
    >>> print url / u'\N{LATIN SMALL LETTER N WITH TILDE}'
    http://example.com/%C3%B1
    >>> url
    <URLObject(u'http://example.com/') at 0x...>
    >>> new_url = url / 'place'
    >>> new_url
    <URLObject(u'http://example.com/place') at 0x...>
    >>> new_url &= 'key', 'value'
    >>> new_url
    <URLObject(u'http://example.com/place?key=value') at 0x...>
    >>> new_url &= 'key2', 'value2'
    >>> new_url
    <URLObject(u'http://example.com/place?key=value&key2=value2') at 0x...>
    >>> new_url |= 'key', 'newvalue'
    >>> new_url
    <URLObject(u'http://example.com/place?key2=value2&key=newvalue') at 0x...>
    
## Important points to note
    
* URLObjects are completely unicode-aware (they subclass `unicode`). This also means that international hostnames will be encoded to IDNA format, and international characters in pathnames will be automatically escaped. You should continue using unicode values for everything; the various components will be en/decoded on-the-fly.

* `url & (key, value)` adds `key=value` to URL, even if `key` is already present as a query parameter. This allows you to have multiple appearances of `key` in the query.

* `url | (key, value)` adds `key=value` to URL, removing any previous appearance of `key` in the query parameters.

* `url & dictionary` and `url | dictionary` work similarly to their `(key, value)` counterparts, only they add every key, value pair in the dictionary to the query string. You can also pass in a list of key, value pairs.

* `url / 'path'` adds `'path'` to the current path, quoting special characters if necessary.

* `url // 'path'` sets the path to `'path'`, removing the current path if present.

* `url * 'fragment'` sets the fragment to `'fragment'`.

* `url ^ 123` sets the port number to `123`.

* `url.with_*(value)` can be done with scheme, host, port, path, query and fragment, returning a new URL object with the value in that place.

* `url.without_port()`, `url.without_path()`, `url.without_query()` and `url.without_fragment()` all exist and do something obvious.

* Operations return a *new* URL object (URL objects are immutable).

## Hints and tips
    
* If a URL's scheme is `'http'` and you try to set the port to 80, it is equivalent to not specifying the port (same goes for `'https'`, `'ftp'` and `'ftps'` for their appropriate ports).

* If you need to end the path with `'/'`, you can do either `url / ''` or `url / 'last_component/'`.

* The query parameters are available as a list through the `query_list()` method and as a dictionary via `query_dict()`. By default, the latter method will return a dictionary with lists as the values, corresponding to potential multiple occurrences of the same key. You can just take the last value by passing the `seq=False` keyword argument to the method.

* Since `URLObject` subclasses directly from Python's built-in `unicode`, you can pass URL objects straight into `urllib2.urlopen()`, JSON serializers, templating systems, etc. If you need a plain-old string or Unicode object, you can just call `str` or `unicode` on it.

## (Un)license

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or distribute this
software, either in source code form or as a compiled binary, for any purpose,
commercial or non-commercial, and by any means.

In jurisdictions that recognize copyright laws, the author or authors of this
software dedicate any and all copyright interest in the software to the public
domain. We make this dedication for the benefit of the public at large and to
the detriment of our heirs and successors. We intend this dedication to be an
overt act of relinquishment in perpetuity of all present and future rights to
this software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org/>
