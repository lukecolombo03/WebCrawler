Our program first logs in to Fakebook, then crawls through the pages until it finds 5 flags. 

In order to log in, we first GET the login page, and start building a POST request. We created an HTML parser in order
to get the csrfmiddlewaretoken from the GET response body. 

We also have a parse_http_response method to parse the response headers, to get the csrftoken and sessionid. This 
method's main purpose is to turn the HTTP headers into a header:value dictionary, but along the way, it sets the 
sessionid and csrftoken (which are fields of Crawler).

Once we create a proper POST request, we send that, and are then redirected to the root of the server, and then begin 
our crawl from the homepage of Fakebook. In our while loop, we first get the first URL in TO_VISIT, then construct
a GET request around that URL. We didn't do keep-alive, so every time we send a request, we create a new socket, read 
from it in a while loop, then close it. Depending on the response code, we do nothing (200), add the location to
TO_VISIT (302), add the URL to NO_VISIT (403, 404), or retry that same URL (503). Then the while loop repeats.

We generally tested our code using print statements. During the login process, we printed out the requests and 
responses to make sure our values were as expected. When creating the HTML and HTTP parsers, we tested those in a 
scratch file, giving them different HTTP and HTML and making sure they were able to identify the important parts. 
Once we got to the crawling process, we would just let it run until it either got an error or all of the flags. We got
a lot of different errors and spent a lot of time tweaking the code, removing certain lines until it would work on 
gradescope.

What we struggled with most for this project was logging in, because there are a lot of moving parts, between the 
cookies, the middleware token, and the HTTP/1.1 headers. We also did not know that the body had to go middlewaretoken,
username, password, in that order, and we kept getting 403 errors because of that. We also got a lot of errors crawling
when we tried to optimize our code, so we ended up going for correctness over performance.