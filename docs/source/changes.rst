=========
Changelog
=========

This is a record of all past mygrad releases and what went into them,
in reverse chronological order. All previous releases should still be available
on pip.

.. _v1.9.0:

------------------
1.9.0 - 2020-08-28
------------------

The most significant aspect of this release is the implementation of ``Tensor.__array__``, which enables a huge amount
of cross-compatibility with numpy utilities (`#288 <https://github.com/rsokl/MyGrad/pull/288>`_). Note that any previous
reliance of a numpy function to produce an array of tensor-scalars will likely produce a standard numpy array instead.

Improvements:

- ``x**1`` and ``x**2`` are now special-cased in order to make these common operations more efficient (`#266 <https://github.com/rsokl/MyGrad/pull/266>`_)
- The derivative of :func:`~mygrad.nnet.losses.focal_loss` was refactored to handle special edge-cases and the tests for focal loss were improved to exercise these edge cases (`#269 <https://github.com/rsokl/MyGrad/pull/269>`_)
- Various improvements to the tests (`#271 <https://github.com/rsokl/MyGrad/pull/271>`_, `#277 <https://github.com/rsokl/MyGrad/pull/277>`_, `#290 <https://github.com/rsokl/MyGrad/pull/290>`_, `#284 <https://github.com/rsokl/MyGrad/pull/284>`_, `#289 <https://github.com/rsokl/MyGrad/pull/289>`_, `#282 <https://github.com/rsokl/MyGrad/pull/282>`_, `#292 <https://github.com/rsokl/MyGrad/pull/292>`_, `#293 <https://github.com/rsokl/MyGrad/pull/293>`_)
- The internal mechanism for tracking tensors in computational graph now depends on hashing tensor-IDs instead of hashing tensors directly. The fact that tensors could be hashed was due to the fact that its equality specialty methods were being monkey-patched (`#276 <https://github.com/rsokl/MyGrad/pull/276>`_)
- :func:`~mygrad.nnet.activations.softmax` and :func:`~mygrad.nnet.activations.logsoftmax` both expose ``axis`` arguments (`#268 <https://github.com/rsokl/MyGrad/pull/268>`_)

Bug fixes:

-  `0D tensors could not be indexed into <https://github.com/rsokl/MyGrad/issues/272>`_ – e.g. to insert a newaxis (`#273 <https://github.com/rsokl/MyGrad/pull/273>`_)
- There was a potential numerical instability in :func:`mygrad.nnet.layers.batchnorm` (`#285 <https://github.com/rsokl/MyGrad/pull/285>`_)
- The ``dtype`` argument in ``Tensor.__init__`` was ignored when the array-like argument, x, was another Tensor-instance (`#294 <https://github.com/rsokl/MyGrad/pull/294>`_)

New features:

- ``Tensor.__array__`` now exposes the tensor's underlying numpy array – this enables a huge amount of cross-compatibility with numpy utilities (`#288 <https://github.com/rsokl/MyGrad/pull/288>`_)
- Adds :func:`~mygrad.asarray` (`#279 <https://github.com/rsokl/MyGrad/pull/279>`_)
- Adds :func:`~mygrad.astensor` (`#294 <https://github.com/rsokl/MyGrad/pull/294>`_)


.. _v1.8.1:

------------------
1.8.1 - 2020-07-28
------------------

This is an `internal change <https://github.com/rsokl/MyGrad/pull/265>`_ to the backprop
mechanism for ``Tensor.__getitem__``, which produces considerable speedups (2x-4x) for backprop
through basic indexing and boolean indexing. Thanks to Petar Griggs for finding this.


.. _v1.8.0:

------------------
1.8.0 - 2020-07-25
------------------

New features:

- Adds :func:`~mygrad.any` and :func:`~mygrad.Tensor.any`
- Adds :func:`~mygrad.random.rand`
- Adds :func:`~mygrad.random.randint`
- Adds :func:`~mygrad.random.randn`
- Adds :func:`~mygrad.random.random`
- Adds :func:`~mygrad.random.random_integers`
- Adds :func:`~mygrad.random.random_sample`
- Adds :func:`~mygrad.random.ranf`
- Adds :func:`~mygrad.random.sample`
- Adds :func:`~mygrad.random.seed`

Thanks to Darshan Krishnaswamy and Sam Carpenter for adding this functionality!

Fixes a bug in the GRU layer where mixed floating point precision dtypes between data and weights raised an error.
Thanks to Petar Griggs for the fix!

.. _v1.7.1:

------------------
1.7.1 - 2020-07-11
------------------

Fixes a bug in :func:`~mygrad.nnet.losses.negative_log_likelihood`, where setting ``constant=True`` had no effect.


.. _v1.7.0:

------------------
1.7.0 - 2020-07-11
------------------

This release continues the process of integrating functions from `mynn <https://github.com/davidmascharka/MyNN>`_.

New features:

- Adds :func:`~mygrad.nnet.initializers.glorot_normal`
- Adds :func:`~mygrad.nnet.initializers.glorot_uniform`
- Adds :func:`~mygrad.nnet.initializers.he_normal`
- Adds :func:`~mygrad.nnet.initializers.he_uniform`
- Adds :func:`~mygrad.nnet.initializers.normal`
- Adds :func:`~mygrad.nnet.initializers.uniform`
- Adds :func:`~mygrad.nnet.losses.focal_loss`
- Adds :func:`~mygrad.nnet.losses.negative_log_likelihood`

Big thanks to David Mascharka!

Improvements:

The interfaces to :func:`~mygrad.reshape` and :func:`~mygrad.Tensor.reshape` were adjusted to match exactly the interfaces to their NumPy counterparts.
I.e. :func:`~mygrad.reshape` now requires ``newshape`` to be a sequence, whereas :func:`~mygrad.Tensor.reshape` can accept an unpacked sequence for its
``newshape``.

:func:`~mygrad.Tensor.shape` is now settable - triggering an in-place reshape of a tensor, matching the corresponding behavior in NumPy.

Internal changes:

The logic for writing an in-place operation has been consolidated into a convenient wrapper: :func:`~mygrad.Tensor._in_place_op`.


.. _v1.6.0:

------------------
1.6.0 - 2020-06-21
------------------

New features:

- Adds :func:`~mygrad.nnet.activations.elu`
- Adds :func:`~mygrad.nnet.activations.glu`
- Adds :func:`~mygrad.nnet.activations.leaky_relu`
- Adds :func:`~mygrad.nnet.activations.selu`
- Adds :func:`~mygrad.nnet.activations.soft_sign`

Big thanks to David Mascharka!


.. _v1.5.0:

-------------------
1.5.0 - 2020-02-16
-------------------

New features:

- Adds :func:`~mygrad.Tensor.astype` method.
- Adds :func:`~mygrad.nnet.activations.hard_tanh`
- ``y_true`` can now be passed as a ``Tensor`` to :func:`~mygrad.nnet.losses.softmax_crossentropy`


This update also includes various improvements to the library's test suite.

.. _v1.4.1:

-------------------
1.4.1 - 2020-01-09
-------------------

This release performs an internal refactor in the ``nnet`` module of the library, as well as
an analogous refactor in the test suite. This also fixes a docstring in the ``multiclass_hinge``
loss to properly show a description in the readthedocs page.

.. _v1.4.0:

-------------------
1.4.0 - 2019-12-19
-------------------

This release adds the :func:`~mygrad.repeat` operation. It also includes some minor
improvements to mygrad's test suite.


.. _v1.3.0:

-------------------
1.3.0 - 2019-11-30
-------------------

This release adds :func:`~mygrad.clip` and :func:`~mygrad.where`.

It also includes a major fix to the graph-traversal mechanism for null-gradients and clear-graph,
eliminating an exponentially-scaling runtime.

``+x`` will now invoke ``mygrad.positive``, mirroring the numpy behavior

There are improvements to user-facing error messages and input validation in addition to major
improvements to mygrad's test suite. There is now a 100% line-coverage gate in mygrad's CI system.


.. _v1.2.0:

-------------------
1.2.0 - 2019-08-03
-------------------

We're finally keeping a formal changelog!

This release makes substantial improvements to MyGrad's error-checking and handling, in order to make much simpler the process of debugging issues with buggy custom operations. Specifically, :func:`~mygrad.operation_base.Operation.backward` now checks for an invalid-gradients on each call of :func:`~mygrad.operation_base.Operation.backward_var`, and raises a descriptive error message.

``mygrad.errors`` was introduced to provide descriptive, MyGrad-specific exceptions. For example, we no longer raise bare exceptions for scenarios like invalid backprop through a scalar-only graph; rather, we now raise a descriptive ``InvalidBackprop`` exception.

MyGrad's testing framework received wide-ranging improvements, yielding complete test coverage and fewer flaky tests. Coverage checks were added to the project's CI process.

:func:`~mygrad.maximum` and :func:`~mygrad.minimum` were patched to permit backpropagation through scalar inputs.

Internal implementation details of :func:`~mygrad.einsum` were adjusted to remove redundant code in its backpropagation machinery.

:func:`~mygrad.Tensor.null_gradients` was refactored to ensure that only a single traversal of the computational graph is performed to null all of the tensors' gradients. Furthermore, `Tensor.null_gradients(clear_graph=True)` now only performs a single graph traversal, instead of two.

In keeping with NumPy's behavior, performing `+x` (where `x` is a mygrad-tensor) no longer returns a reference of `x`, but returns `mygrad.positive(x)`.

Backpropagation through :func:`~mygrad.max` and :func:`~mygrad.min` now works for 0D tensors.

Input validation was added to :func:`mygrad.nnet.layers.utils.sliding_window_view`.

Fixed backpropagation through basic indexing, `x[ind] = b`, in which broadcasting occurred and `b` possess "excess" leading singleton dimensions.

