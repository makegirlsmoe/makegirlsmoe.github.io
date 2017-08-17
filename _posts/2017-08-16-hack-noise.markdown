---
layout: default
title:  "Web Interface Update: Hack the Noise and More"
date:   2017-08-16 20:51:00
categories: main
---

# Web Interface Update: Hack the Noise and More

In order to have a more interesting user experience, we introduce some updates to our [web interface](http://make.girls.moe).


#### For People who Want to Hack the Noise:

With growing interest in playing with and manipulating the noise
([here](http://kaibutugirl.hatenablog.jp/entry/2017/08/16/012434), [here](https://twitter.com/prateamsy/status/897819679439241216),
and [many more](https://twitter.com/hashtag/MakeGirlsMoe)),
we here introduce a more direct (and somewhat hackish) way to control over the noise. You can specify the noise by importing your own noise image. The noise image can be of common formats (such as PNG, JPG, etc.), and must be colored, with the resolution of $$128 \times 34$$ ($$128$$ columns by $$34$$ rows). In the process of importing noise, only the first row of the image is used, and the rest rows are ignored, as shown below:

<center><img src="{{ site.url }}/assets/news-img/noise-explain.png" align="middle" width="500"></center>

The first row, consisting of $$128$$ pixels, specifies the $$128$$-length noise vector $$v$$. The $$ i $$-th element of the noise vector ($$ v_i $$) is calculated using

$$  v_i = \sqrt{ -2  \log (1 - B_{i} / 256) } \cdot \cos{ ( 2 \pi (1 - G_{i} / 256) ) }  $$

with $$ G_{i} $$ and $$ B_{i} $$ are the values of the G channel and the B channel of the $$ i $$-th pixel respectively. The R channel (and the A channel, if exists) is/are ignored.

PS: A noise vector with elements near $$0$$ usually generates good images.

#### ...and More: UI Updates

We also provide a twitter stream of our [#MakeGirlsMoe](https://twitter.com/hashtag/MakeGirlsMoe) on the web site --- you can easily see what others are generating and sharing!
