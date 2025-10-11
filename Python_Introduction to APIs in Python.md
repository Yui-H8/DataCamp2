# Introduction to APIs in Python
---
### API requests with urllib
For this course, you will be using the API for a Music Catalog application. This API has multiple features. You will start with the Lyrics API, which allows you to retrieve a quote from the Lyric of the day.

Before you can make your first API request, you will need to know where the API can be accessed. This location is also referred to as the URL, short for Uniform Resource Locator. The URL will tell Python where to send the API request to. The URL for the Lyrics API is as follows: http://localhost:3000/lyrics/.

Let's make a first request to the Lyrics API using the built-in urllib Python module.
* Use the read function on the response object to read the response data from the response object.
* Use the decode function to decode the response data into a string with the right encoding.
```python
from urllib.request import urlopen

with urlopen('http://localhost:3000/lyrics/') as response:
  
  # Use the correct function to read the response data from the response object
  data = response.read()
  encoding = response.headers.get_content_charset()

  # Decode the response data so you can print it as a string later
  string = data.decode(encoding)
  
  print(string)
```
> N' I never miss Cause I'm a problem child - AC/DC, Problem Child
