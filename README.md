# Matryoshka GPT
the idea is based on matryoshka embeddings from [this paper](https://arxiv.org/abs/2205.13147) which are a hierarchical representation scheme designed for representation learning that allowed one representation model to train embedding vectors of varying sizes simultaneously that also fit inside each other, like russian nesting dolls. see my youtube explanation below:

[![click me for video](https://img.youtube.com/vi/dUeM_yDuGbg/0.jpg)](https://www.youtube.com/watch?v=dUeM_yDuGbg)

in `matryoshka_embeddings_gpt.ipynb` i've implemented matryoshka embeddings into a GPT model, meaning that for the final output residual `x` of shape `(b,t,d)` and transposed embedding matrix output of shape `(d,v)` you have the option to slice off the `d` dimension at various sizes that are all powers of 2, and the model will still work. this isn't really that useful for just a GPT because it doesn't actually save any significant compute. It does however give you embeddings that are self-similar at different spliced sizes, which would be useful if all you cared about was representation learning from a language mode.


in `matryoshkaGPT.ipynb` I'm making the entire model exhibit the same splicing behavior within the inner-workings of the GPT, for example the kv multiplication using these smaller length `d`'s and corresponding smaller head sizes. this has already been done by [MATFORMER](https://arxiv.org/pdf/2310.07707.pdf) except they only implmenented it on the feedforward parts, no the MHA. I'd like to have it apply to literally every part of the model

- So far I've coded up the feedforward network, the self-attention heads, the token embeddings & positional encodings. still to do are the multi-head attention concatenation, the residual blocks, and the generate() function used for inference which I think will be a nightmare. So far though it all looks very doable

- one problem with this approach is that we're only really creating this "nesting doll" effect along one model dimension, `d`. Your smaller nesting doll models will still have the same number of layers and attention heads per MHA mechanism. I'm currently hypothesizing as to how I could fix this and have the smaller models really be smaller along all dimensions such that they're optimal in terms of their hyperparameters, but let's not get too ahead of ourselves

the models pertaining to imposed & emergent hierarchical embeddings have been moved to [this repo](https://github.com/evintunador/hierarchical_embeddings.git)
