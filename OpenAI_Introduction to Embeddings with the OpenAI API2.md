# Introduction to Embeddings with the OpenAI API (2)
---
### More repeatable embeddings
As you continue to work with embeddings, you'll find yourself making repeated calls to OpenAI's embedding model. To make these calls in a more repeatable and modular way, it would be better to define a custom function called create_embeddings() that would output embeddings for any number of text inputs. In this exercise, you'll do just that!
* Define a create_embeddings() function that sends an input, texts, to the embedding model, and returns a list containing the embeddings for each input text.
* Embed short_description using create_embeddings(), and extract and print the embeddings in a single list.
* Embed list_of_descriptions using create_embeddings() and print.
> Hint  
> Recall that the list of embeddings for each input is stored under the 'data' key of response_dict.    
> To extract the embedded short_description as a single list, make sure to zero-index before printing.
