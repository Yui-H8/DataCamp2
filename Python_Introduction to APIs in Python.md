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
> N' I never miss Cause I'm a problem child - AC/DC, Problem Child

*Nice, using the requests package you have simplified the code a lot. The requests package even took care of decoding the response for you, how cool is that? Let's move on and see what more can be done with the requests package.*

### The basic anatomy of an API request
---
### The 4 most important HTTP Verbs
HTTP verbs are a fundamental concept in REST APIs. There are 9 verbs in total, you got to know the four most important ones used by REST APIs: GET, POST, PUT and DELETE.

Previously you learned how you can perform actions on API resources using these HTTP verbs. Each verb is associated with a specific type of action. Do you know what action belongs to what verb?

Note: For the purpose of this exercise we only use the three most common verbs: GET, POST and DELETE.

### Constructing a URL with parameters
You can fine-tune your API requests using the path and query parameters of the URL. Let's learn how you can use HTTP verbs, URL paths, and parameters using the requests package.

In this exercise, you will make another API request to the Lyrics API, but instead of getting today's lyric, you will send a request to the random lyrics API. You will then further customize the API request by adding query parameters to filter on specific artists and include the track title. Below, you can find the details needed to construct the correct URL.

1. Construct the URL to the random lyrics API for the requests.get() method using the protocol, domain, port and path components.    
Note: Do not use the query parameters yet, we will add these in the next steps!
```python
# Construct the URL string and pass it to the requests.get() function
response = requests.get('http://localhost:3000/lyrics/random')

print(response.text)
```
2. Let's now add a query parameter to only get lyrics by a specific artist.
* Create a dictionary variable with one entry: the key artist with a value of Deep Purple.
* Pass this dictionary to the requests.get() method as the params argument.
```python
# Create a dictionary variable with query params
query_params = {'artist':'Deep Purple'}

# Pass the dictionary to the get() function
response = requests.get('http://localhost:3000/lyrics/random', params=query_params)

print(response.text)
```
> output:     You're racing like a fireball - Deep Purple

3. Add a second item to the dictionary with the key include_track and the Boolean value True.
* Print the response's url attribute to see the full URL.
* Print out the lyric.
```python
# Add the `include_track` parameter
query_params = {'artist': 'Deep Purple', 'include_track': True}

response = requests.get('http://localhost:3000/lyrics/random', params=query_params)

# Print the response URL
print(response.url)

# Print the lyric
print(response.text)
```
> http://localhost:3000/lyrics/random?artist=Deep+Purple&include_track=True
> You're racing like a fireball - Deep Purple, Fireball

*Awsome, now the track title is included in the lyric! Check the URL we printed, notice how the requests library took care of properly encoding and structuring the artists query parameter for you? Cool isn't it?*

### Creating and deleting resources using an API
Now that you have learned how to construct a URL, you can send requests to specific API resources. Let's see what more you can do with HTTP verbs on these resources.

In this exercise, you will use the playlists API available via http://localhost:3000/playlists/. This API offers the following actions:
| Verb   | Path                        | Description                                                              |
|--------|-----------------------------|--------------------------------------------------------------------------|
| GET    | /playlists                  | get a list of all playlists                                              |
| GET    | /playlists/{PlaylistId}     | get information on a single playlist using its unique identifier PlaylistId |
| POST   | /playlists                  | create a new playlist                                                    |
| DELETE | /playlists/{PlaylistId}     | remove an existing playlist using its unique identifier PlaylistId       |

You will start by getting a list of all existing playlists, then you will learn how to create a new playlist and verify it's creation, and last you will learn how to remove an existing playlist.

The requests library is already imported for your convenience.
1. Get a list of all playlists from the playlists API.
```Python
# Get a list of all playlists from the API
response = requests.get('http://localhost:3000/playlists')
print(response.text)
```

> output:    [{"PlaylistId":1,"Name":"Classical music"},{"PlaylistId":2,"Name":"Pop"}]


```python
# Create a dictionary with the playlist info
playlist_data = {"Name": "Rock Ballads"}

# Perform a POST request to the playlists API with your dictionary as data parameter
response = requests.post('http://localhost:3000/playlists', data=playlist_data)
print(response.text)
```
