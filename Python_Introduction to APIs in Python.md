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

*Nice, using the urllib module you have retrieved the 'Lyric of the day' from the Lyric API. The code is quite complex though, as we had to deal with decoding, etc. Let's see if we can make it simpler.*

### Using the requests package
Using urllib to integrate APIs can result in verbose and complex code as you need to take care of a lot of additional things like encoding and decoding responses.

As an alternative to urllib, the requests Python package offers a simpler way to integrate APIs. A lot of functionality is available out of the box with requests, which makes your code a lot easier to write and read. Let's try the same exercise again but now with the requests package.

Remember, as with the previous exercise, the URL for the Lyrics API is http://localhost:3000/lyrics.
* Import the requests package.
* Pass the URL http://localhost:3000/lyrics to the requests.get method.
* Print out the response text.
```python
# Import the requests package
import requests

# Pass the API URL to the get function
response = requests.get('http://localhost:3000/lyrics')

# Print out the text attribute of the response object
print(response.text)
```
