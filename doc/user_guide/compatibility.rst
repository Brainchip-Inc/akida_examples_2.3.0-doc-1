Akida versions compatibility
============================

This document is for users who need backwards compatibility across different
versions of Akida either for code or models.


Upgrading models with legacy quantizers
---------------------------------------

The ``WeightQuantizer`` and ``TrainableWeightQuantizer`` objects from the
CNN2SNN tool have been renamed to ``StdWeightQuantizer`` and
``TrainableStdWeightQuantizer`` in version **1.8.10**.

.. note:: ``TrainableStdWeightQuantizer`` is deprecated since version 2.1.0.

This introduces an incompatibility as models containing object with deprecated
names will not be recognised anymore and thus cannot be loaded using
`cnn2snn.load_quantized_model <../api_reference/cnn2snn_apis.html#load-quantized-model>`_.

In fact, loading such models would lead to the following error:

.. code-block:: python

    from cnn2snn import load_quantized_model
    model = load_quantized_model('model_with_legacy_quantizers.h5')

    ValueError: Unknown object: WeightQuantizer

Upgrading legacy models is done thru the CNN2SNN CLI ``upgrade`` action:

.. code-block:: bash

    cnn2snn upgrade -m 'model_with_legacy_quantizers.h5' -o 'upgraded_model.h5'

.. note:: The ``upgrade`` CLI and associated API as been deprecated in version
        2.1.0. If still using legacy models, you should use intermediate
        versions to upgrade them.

This command will save an ``upgraded_model.h5`` file that can be loaded with
latest Akida tools.

.. note::
    While two model formats can be upgraded: h5/hdf5 Keras format and
    tf/pb format ; all information (compile, training, ...) is preserved when
    upgrading h5/hdf5 models but only configuration and weights are preserved
    when handling tf/pb models.
