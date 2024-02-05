# Matryoshka GPT
the idea is based on matryoshka embeddings from [this paper](https://arxiv.org/abs/2205.13147) which are a hierarchical representation scheme designed for representation learning that allowed one representation model to train embedding vectors of varying sizes simultaneously that also fit inside each other


in this project i've implemented matryoshka embeddings into a GPT model, meaning that for the final output residual `x` of shape `(b,t,d)` and transposed embedding matrix output of shape `(d,v)` you have the option to slice off the `d` dimension at various sizes that are all powers of 2, adn the model will still work. this isn't really that useful for just a GPT because it doesn't actually save any significant compute. However, my two goals are
1. see if i can make this relate to hierarchical knowledge graph embeddings
    1a. can i get the model to dynamically present to us hierarchies of tokens? for example, if we're doing character-wise tokenization, then i want the smaller embedding dimension lengths to correspond to categories like "captial vs lowercase letters" or "vowels vs consonants" which is already something that i can clearly see in a cosine similarity display of the embeddings
    1b. can i force a pre-determined hierarchial structure on the embeddings? So for this case, rather than the model figuring out what vowels and consonants are on its own, i'd be imposing that categorization on the model
2. see if i can figure out a way to make the model exhibit the same splicing behavior within the inner-workings of the GPT, for example the kv multiplication using these smaller length `d`'s. if i can figure this out then it'd mean that whenever you train a matryoshkaGPT, you're also training many far smaller models that could be used instead if your inference is constrained


currently i've only started to scratch the surface of thinking about how i might do these two things