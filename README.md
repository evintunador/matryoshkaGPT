# Matryoshka Representation Learning
`matryoshka_embeddings_gpt.ipynb` is a replication of matryoshka embeddings from [this paper](https://arxiv.org/abs/2205.13147) which are a hierarchical representation scheme designed for representation learning that allowed one representation model to train embedding vectors of varying sizes simultaneously that also fit inside each other, like russian nesting dolls. see my youtube explanation below:

[![click me for video](https://img.youtube.com/vi/dUeM_yDuGbg/0.jpg)](https://www.youtube.com/watch?v=dUeM_yDuGbg)

# MatryoshkaGPT
in `matryoshkaGPT.ipynb` I'm made the entire model exhibit the same splicing behavior as above within the inner-workings of the GPT, for example the kv multiplication using these smaller `d` lengths and corresponding smaller head sizes. this has already been done by [MATFORMER](https://arxiv.org/pdf/2310.07707.pdf) except they only implmenented it on the feedforward network, not the MHA, whereas I've done it with literally every part of the model

[![click me for video](https://img.youtube.com/vi/dIvqtkM_bCw/0.jpg)](https://www.youtube.com/watch?v=dIvqtkM_bCw)

- one problem with this approach is that we're only really creating this "nesting doll" effect along one model dimension, `d`. Your smaller nesting doll models will still have the same number of layers and attention heads per MHA mechanism. I'm currently hypothesizing as to how I could fix this and have the smaller models really be smaller along all dimensions such that they're optimal in terms of their hyperparameters, but let's not get too ahead of ourselves

if you're looking for the models pertaining to imposed & emergent hierarchical embeddings, they have been moved to [this repo](https://github.com/evintunador/hierarchical_embeddings.git)

p.s.
the code in this repo is based on andrej karpathy's minGPT