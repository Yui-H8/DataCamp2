# Introduction to Embeddings with the OpenAI API (2)
---
### More repeatable embeddings
As you continue to work with embeddings, you'll find yourself making repeated calls to OpenAI's embedding model. To make these calls in a more repeatable and modular way, it would be better to define a custom function called create_embeddings() that would output embeddings for any number of text inputs. In this exercise, you'll do just that!
