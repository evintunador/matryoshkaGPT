# Matryoshka GPT
the idea is based on matryoshka embeddings from [this paper](https://arxiv.org/abs/2205.13147) which are a hierarchical representation scheme designed for representation learning that allowed one representation model to train embedding vectors of varying sizes simultaneously that also fit inside each other, like russian nesting dolls


in `matryoshka_embeddings_gpt.ipynb` i've implemented matryoshka embeddings into a GPT model, meaning that for the final output residual `x` of shape `(b,t,d)` and transposed embedding matrix output of shape `(d,v)` you have the option to slice off the `d` dimension at various sizes that are all powers of 2, and the model will still work. this isn't really that useful for just a GPT because it doesn't actually save any significant compute. It does however give you embeddings that are self-similar at different spliced sizes, which would be useful if all you cared about was representation learning from a language mode.


Next, I've got two ideas that I think I can implement

1. see if i can make this relate to hierarchical knowledge graph embeddings
    
    1a. in `imposed_hierarchical_embeddings_GPT.ipynb` I'm trying to force a pre-determined hierarchial structure onto the embeddings. The idea here is that hopefully I can force a model to think in terms of human-defined hierarchical conceptual categories. If those are good categories and I'm very lucky, then the model may even be able to train faster.
       - so far I've got it all up & running with a model trained and some example images. It does in fact seem like i can force structure on the embeddings at least to some degree. most of the work to be done now has to do with hyperparameter tuning and trying it on a slightly larger model. Here's an image example, where "degree" means the length of the embedding subset

<p align="center">
<img src="./images/imposed_hierarchical_embeddings_GPT_b4_t16_d32_h4_l4_lr0.0003_drop0.2_l2-0.01_min_power1_2024-02-07|23-21-13_symbolsvsletters.png" width="512"/>
</p>

<p align="center">
<img src="./images/imposed_hierarchical_embeddings_GPT_b4_t16_d32_h4_l4_lr0.0003_drop0.2_l2-0.01_min_power1_2024-02-07|23-22-20_endofsentencevsmidsentence&uppercasevslowercase.png" width="512"/>
</p>


1b. in `emergent_hierarchical_embeddings_GPT.ipynb` I've gotten the model to dynamically present to us hierarchies of tokens that it learns through training. for example, if we're doing character-wise tokenization, then i want the smaller embedding dimension lengths to naturally correspond to categories like "captial vs lowercase letters" or "vowels vs consonants" which is already something that i can clearly see in a cosine similarity display of `matryoshka_embeddings_gpt.ipynb`. The difficulty here will be in picking good a good clustering algorithm, good hyperparameters for that clustering algorithm, and then implementing these categories at the right speed during training. As of now my clustering methodology is just setting a minimum cosine similarity level and group size, but i'd like to progress this to something more sophisticated. Here are some examples of the very promising results so far

<p align="center">
<img src="./images/emergent_hierarchical_embeddings_GPT_b8_t24_d32_h4_l4_lr0.0003_drop0.2_l2-0.01_min_power2_2024-02-08|20-22-47_V2_thispower2.png" width="512"/>
</p>

<p align="center">
<img src="./images/emergent_hierarchical_embeddings_GPT_b8_t24_d32_h4_l4_lr0.0003_drop0.2_l2-0.01_min_power2_2024-02-08|20-22-49_V2_thispower3.png" width="512"/>
</p>

2. in `matryoshkaGPT.ipynb` I'm trying to see if i can figure out a way to make the model exhibit the same splicing behavior within the inner-workings of the GPT, for example the kv multiplication using these smaller length `d`'s and corresponding smaller head sizes. if i can figure this out then it'd mean that whenever you train a matryoshkaGPT, you're also training many far smaller models that could be used instead if your inference is constrained. Imagine it like you didn't know what russian nesting dolls were, got a set of them with no paint on them, painted the outside one, and then once you were done you opened the biggest doll to find that all the dolls inside are also painted to your style, albiet progressively sloppier as you look at smaller & smaller nesting dolls.
    - So far I've coded up the feedforward network, the self-attention heads, the token embeddings & positional encodings. still to do are the multi-head attention concatenation, the residual blocks, and the generate() function used for inference which I think will be a nightmare. So far though it all looks very doable
    - one problem with this approach is that we're only really creating this "nesting doll" effect along one model dimension, `d`. Your smaller nesting doll models will still have the same number of layers and attention heads per MHA mechanism. I'm currently hypothesizing as to how I could fix this and have the smaller models really be smaller along all dimensions such that they're optimal in terms of their hyperparameters, but let's not get too ahead of ourselves
    - THIS PROJECT IS ON HOLD. I DID NOT REALIZE THAT THE AUTHORS OF THE MATRYOSHKA EMBEDDINGS PAPER HAVE ALREADY DONE THIS. SEE [MATFORMER](https://arxiv.org/pdf/2310.07707.pdf). I WILL EXPLORE THEIR CODE AND SEE WHETHER I STILL HAVE ANYTHING NOVEL TO ADD