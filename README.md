# Matryoshka Representation Learning
`MRL.ipynb` is a replication of matryoshka embeddings from [this paper](https://arxiv.org/abs/2205.13147) which are a hierarchical representation scheme designed for representation learning that allowed one representation model to train embedding vectors of varying sizes simultaneously that also fit inside each other, like russian nesting dolls. see my youtube explanation below:

[![click me for video](https://img.youtube.com/vi/dUeM_yDuGbg/0.jpg)](https://www.youtube.com/watch?v=dUeM_yDuGbg)

# MatFormer+
in `MatFormer+.ipynb` I'm made the entire model exhibit the same splicing behavior as above within the inner-workings of the GPT, for example the kv multiplication using these smaller `d` lengths and corresponding smaller head sizes. this has already been done by [MATFORMER](https://arxiv.org/pdf/2310.07707.pdf) except they only implmenented it on the feedforward network, not the MHA, whereas I've done it with literally every part of the model

[![click me for video](https://img.youtube.com/vi/dIvqtkM_bCw/0.jpg)](https://www.youtube.com/watch?v=dIvqtkM_bCw)

# MatryoshkaGPT
in `MatryoskhaGPT.ipynb` i'm incorporating the ideas from [this paper](https://arxiv.org/pdf/2402.14776.pdf) to make MatFormer+ not only subsettable in all weight matrices but also across layers. Basically if you don't want to use all $L$ layers you can just cut the model off at your desired $l$ and multiply by the final output matrix. This is just one more element of matryoshka-ness. Not sure if Imma bother doing a video on this one

# Misc
- `tangents.ipynb` is some rant I was going on at one point that's somehow related
- if you're looking for the models pertaining to imposed & emergent hierarchical embeddings, they have been moved to [this repo](https://github.com/evintunador/hierarchical_embeddings.git)
- p.s. the code in this repo is based on andrej karpathy's minGPT