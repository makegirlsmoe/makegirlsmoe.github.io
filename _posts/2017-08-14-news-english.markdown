---
layout: default
title:  "Create Anime Characters with A.I. !"
date:   2017-08-14 06:03:00
categories: main
---

# Create Anime Characters with A.I. !

We all love anime characters and are tempted to create our custom ones,
but most of us simply cannot do that just because we are not pros.
What if anime characters can be automatically generated in a professional level quality?
Image that you just specify some attributes such as blonde/twin tail/smile and an anime character with your customization is generated whiteout any further intervention!

Already we have some pioneers in the anime generation, such as [ChainerDCGAN](https://github.com/pfnet-research/chainer-gan-lib), [Chainerを使ってコンピュータにイラストを描かせる](http://qiita.com/rezoolab/items/5cc96b6d31153e0c86bc), and online-available code such as [IllustrationGAN](https://github.com/tdrussell/IllustrationGAN) and [AnimeGAN](https://github.com/jayleicn/animeGAN).
But very often results generated these models are [blurred](https://github.com/jayleicn/animeGAN/blob/master/images/fake_sample.png)] and [distorted](https://qiita-image-store.s3.amazonaws.com/0/61296/7838e32d-1ca9-be96-ddd9-2e400be99ea1.jpeg),
and it is still a challenge to generate industry-standard facial images for anime characters. As a step towards tackling this challenge, we propose a model that produces anime faces at high quality with promising rate of success.

#### Dataset: a Model of Good Quality Begins with a Clean Dataset.

To teach computer to do things requires high quality data, and our case is not an exception.
Large scale image boards like
[Danbooru](https://danbooru.donmai.us) and [Safebooru](https://safebooru.org) are noisy and we think this is at least partial reason for issues in previous works,
so instead we use standing pictures ([立ち絵](http://dic.nicovideo.jp/a/%E7%AB%8B%E3%81%A1%E7%B5%B5)) from games sold on [Getchu](www.getchu.com), a website providing information and selling of Japanese games.
Standing pictures are diverse enough since they are of different styles for games in a diverse sets of theme, yet consisting since they are all belonging to domain of character images.
We also need categorical metadata (a.k.a tags/attributes) of images like hair color, whether smiling or not.
Getchu does not provide such metadata, so we use [Illustration2Vec](saito2015illustration2vec), a [CNN](https://cs231n.github.io/convolutional-networks/)-based tool for estimating tags of anime.

#### Model: The Essential Part

A good generative model is also a must-have for our goal.
The generator should know and follow user's specified attributes, which is called _prior_,
and should also have freedom to generate different, detailed visual features, which is modeled using _noise_.
We use a popular framework called GAN ([Generative Adversarial Networks](https://papers.nips.cc/paper/5423-generative-adversarial-nets)).
GAN uses a generator network $$G$$ to generate images from _prior_ and _noise_,
and also a another network $$D$$ trying to distinguish $$G$$'s generated images from real images.
We train them, and in the end $$G$$ should be able to generate images so realistic that $$D$$ cannot differentiate them from real images with that _prior_.
However it is infamously hard and time-consuming to properly train GAN.
Luckily a recent advance, named [DRAGAN](https://arxiv.org/abs/1705.07215),
can give presumable results compare to other GANs with least computation power required.
We successfully train the DRAGAN whose generate is [SRResNet](https://arxiv.org/abs/1609.04802)-like.
Also, we need our generator to know the label information so user's customization can be used. Inspired by [ACGAN](https://arxiv.org/abs/1610.09585),
we feed the labels to the generator $$G$$ along with noise and add a multi-label classifier on the top of discriminator $$D$$, which is asked to predict the assigned tags for the images.

With the data and model, we train it straightforwardly with [GPU-powered](http://www.nvidia.com/object/machine-learning.html) machines.

#### Samples: A Picture is Worth a Thousand Words

To taste the quality of our model, see the generated images like the following: it handles different attributes and visual features well.

<center><img src="{{ site.url }}/assets/news-img/samples.jpg" align="middle" width="500"></center>

One interesting setting would be fixing the random _noise_ part and sampling random _priors_. The model now is required to generate images have similar major visual features with different attribute combinations, and it does well:

<center><img src="{{ site.url }}/assets/news-img/fixed_noise.jpg" align="middle" width="500"></center>

Also, by fixing _priors_ and sampling randomly the _noise_, the model can generate images have the same attributes with different visual features:

<center><img src="{{ site.url }}/assets/news-img/fix_attributes_a.png" align="middle" width="500"></center>

#### Web Interface: Bring Neural Generator to your Browser

In order to make our model more accessible, we build a [website interface](http://make.girls.moe) with [React.js](https://facebook.github.io/react/) for open access.
We make the generation completely done on the browser side, by imposing [WebDNN](https://mil-tokyo.github.io/webdnn/) and converting the trained [Chainer](https://chainer.org/) model to the [WebAssembly](http://webassembly.org/)-based Javascript model.
For a better user experience, we would like to keep the size of generator model small since users need to download the model before generating,
and our choice of  [SRResNet](https://arxiv.org/abs/1609.04802) generator make the model $$4$$ times smaller than the popular [DCGAN](https://arxiv.org/abs/1511.06434) generator without compromising on the quality of results.
Speed-wise, even all computations are done on the client side, it takes about only $$6\sim 7$$ seconds to generate one images on average.


#### For more technical details, check out the [technical report](http://make.girls.moe/technical_report.pdf).
