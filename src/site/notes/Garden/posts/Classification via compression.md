---
{"dg-publish":true,"permalink":"/garden/posts/classification-via-compression/","tags":["machine-learning"],"created":"2026-03-10 10:30","updated":"2026-04-04 16:11","dg-note-properties":{"modified_date":"2026-04-04 16:11","creation_date":"2026-03-10 10:30","tags":["machine-learning"]}}
---

# Classification via compression

topic: [[02-Area/programming/AI/Machine Learning\|Machine Learning]]

source: [Text classification with Python 3.14's zstd module • Max Halford](https://maxhalford.github.io/blog/text-classification-zstd/)

In general, text is compressed with zstd with custom dictionary. The dictionary is the target text that you want to search similarity against. Then go through all the text corpus, compress them, and see which one has the smallest size.

It is exploiting the fact that the more similar it is for the content of the compression, the smaller the size will be, due to the dictionary given.

[“Low-Resource” Text Classification: A Parameter-Free Classification Method with Compressors - ACL Anthology](https://aclanthology.org/2023.findings-acl.426/)

> Deep neural networks (DNNs) are often used for text classification due to their high accuracy. However, DNNs can be computationally intensive, requiring millions of parameters and large amounts of labeled data, which can make them expensive to use, to optimize, and to transfer to out-of-distribution (OOD) cases in practice. In this paper, we propose a non-parametric alternative to DNNs that’s easy, lightweight, and universal in text classification: a combination of a simple compressor like gzip with a k-nearest-neighbor classifier. Without any training parameters, our method achieves results that are competitive with non-pretrained deep learning methods on six in-distribution datasets.It even outperforms BERT on all five OOD datasets, including four low-resource languages. Our method also excels in the few-shot setting, where labeled data are too scarce to train DNNs effectively.

This is also applicable to image classification.

[78% MNIST accuracy using GZIP in under 10 lines of code. \| Jakob Serlier's Personal Site](https://jakobs.dev/solving-mnist-with-gzip/)


Key principles
- Normalised Compression Distance (NCD): measures similarity between two objects based on how much more effort it takes to compress them together compared to compressing them separately

Related:
- [The Bitter Lesson is coming for Tokenization \| ⛰️ lucalp](https://lucalp.dev/bitter-lesson-tokenization-and-blt/)