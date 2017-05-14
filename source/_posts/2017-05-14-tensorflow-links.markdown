---
layout: post
title: "Tensorflow links"
date: 2017-05-14 16:27:49 +0200
comments: true
categories: tensorflow python
---

### Bookmarks overflow

- [Setting all negative values of a tensor to zero (in tensorflow)][1]

        X_with_negatives_set_to_zero = tf.nn.relu(X)
        X = tf.cast(X > 0, X.dtype) * X # X > 0 returns a boolean tensor
- Queue code: [How to filter tensor from queue based on some predicate in tensorflow?][2]
- [rmse cost function][3]

        tf.sqrt(tf.reduce_mean(tf.squared_difference(Y1, Y2)))

  but see comments in that answer
- [MNIST Tensorflow vs code from Michael Nielsen][4] - a mess
- [regularization][5] - [notes on biases][6]
- [TensorFlow - numpy-like tensor indexing][7] - `tf.gather_nd` example
- [How can I run a loop with a tensor as its range? (in tensorflow)][8] - `tf.while_loop`
- [TensorFlow: using a tensor to index another tensor][9] - `tf.gather` example - is it a special case of `gather_nd` ?
- [`scatter_nd`][10] - needs answer, as [this one][11]
- [Conditional assignment of tensor values in TensorFlow][12] - `tf.where` and co
- [Set k-largest elements of a tensor to zero in TensorFlow][13] - some C++ code too


  [1]: http://stackoverflow.com/a/41045825/281545
  [2]: http://stackoverflow.com/a/33904534/281545
  [3]: http://stackoverflow.com/a/43830200/281545
  [4]: http://stackoverflow.com/q/37896853/281545
  [5]: http://stackoverflow.com/q/38286717/281545
  [6]: http://stackoverflow.com/questions/38286717/tensorflow-regularization-with-l2-loss-how-to-apply-to-all-weights-not-just#comment69573180_38287616
  [7]: http://stackoverflow.com/a/41893288/281545
  [8]: http://stackoverflow.com/a/39010125/281545
  [9]: http://stackoverflow.com/a/35847836/281545
  [10]: http://stackoverflow.com/a/34686952/281545
  [11]: http://stackoverflow.com/a/39218426/281545
  [12]: http://stackoverflow.com/a/39047066/281545
  [13]: http://stackoverflow.com/q/37593960/281545