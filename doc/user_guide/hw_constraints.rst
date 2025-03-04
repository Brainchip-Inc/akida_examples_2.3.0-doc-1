
Hardware constraints
====================

While working with CNN2SNN and the Akida simulator, only few limitations are
imposed. When mapping a model to the Akida hardware, not all Model and Layer
configurations are supported.

When dealing with an unsupported model, one can call `the dedicated Akida helper
<../api_reference/akida_apis.html#akida.compatibility.create_from_model>`_
that will try to find workarounds for incompatibilities and build a converted
model:

.. code-block::

   import akida.compatibility
   updated_model = akida.compatibility.create_from_model(incompatible_model)


InputConvolutional
^^^^^^^^^^^^^^^^^^

+--------------+---------+----------+------------+
|**Input**     |**Width**|**Height**|**Channels**|
+--------------+---------+----------+------------+
|**Dimensions**|[5:256]  |>= 5      |1, 3        |
+--------------+---------+----------+------------+

+----------------+---------------+----------+-----------+
|**Convolutions**|**Kernel Size**|**Stride**|**Type**   |
+----------------+---------------+----------+-----------+
|**Parameters**  |3×3, 5×5, 7×7  |1, 2, 3   |Same, Valid|
+----------------+---------------+----------+-----------+

+-------------+-------+-------+-------+
|**Filters**  |**3x3**|**5x5**|**7x7**|
+-------------+-------+-------+-------+
|**Max(1 ch)**|512    |192    |96     +
+-------------+-------+-------+-------+
|**Max(3 ch)**|192    |64     |32     +
+-------------+-------+-------+-------+

+---------------+------------------+
|**Max Pooling**|**Size**          |
+---------------+------------------+
|**Parameters** |1x1, 1×2, 2×1, 2×2|
+---------------+------------------+

.. note::
       * pool_stride is equal to pool_size for InputConvolutional

+----------------+---------+-----------+--------------+
|**Quantization**|**Input**|**Weights**|**Activation**|
+----------------+---------+-----------+--------------+
|**Bitwidth**    |8        |1, 2, 4, 8 |1, 2, 4       |
+----------------+---------+-----------+--------------+

Convolutional
^^^^^^^^^^^^^

+----------------+------------------+----------+--------+
|**Convolutions**|**Kernel Size**   |**Stride**|**Type**|
+----------------+------------------+----------+--------+
|**Parameters**  |1x1, 3×3, 5×5, 7×7|1, 2      |Same    |
+----------------+------------------+----------+--------+

.. note::
       * stride 2 is only supported with 3x3 kernels

+---------------+-------------+----------+
|**Max Pooling**|**Size**     |**Stride**|
+---------------+-------------+----------+
|**Parameters** |1x1, 2×2     |1, 2      |
+---------------+-------------+----------+

.. note::
       * pooling stride cannot be greater than pooling size
       * a layer with max pooling must be followed by another Convolutional or
         SeparableConvolutional layer.

+--------------------------+---------+
|**Global Average Pooling**|**Width**|
+--------------------------+---------+
|**Dimensions**            |[1:32]   |
+--------------------------+---------+

.. note::
       * with global average pooling the output of the convolution must have at
         least 3 rows.

+----------------+---------+-----------+--------------+
|**Quantization**|**Input**|**Weights**|**Activation**|
+----------------+---------+-----------+--------------+
|**Bitwidth**    |1, 2, 4  |1, 2, 4    |1, 2, 4       |
+----------------+---------+-----------+--------------+

SeparableConvolutional
^^^^^^^^^^^^^^^^^^^^^^

+----------------+---------------+----------+--------+
|**Convolutions**|**Kernel Size**|**Stride**|**Type**|
+----------------+---------------+----------+--------+
|**Parameters**  |3×3, 5×5, 7×7  |1, 2      |Same    |
+----------------+---------------+----------+--------+

.. note::
       * stride 2 is only supported with 3x3 kernels

+---------------+-------------+----------+
|**Max Pooling**|**Size**     |**Stride**|
+---------------+-------------+----------+
|**Parameters** |1x1, 2×2     |1, 2      |
+---------------+-------------+----------+

.. note::
       * pooling stride cannot be greater than pooling size.
       * a layer with max pooling must be followed by another Convolutional or
         SeparableConvolutional layer.

+--------------------------+---------+
|**Global Average Pooling**|**Width**|
+--------------------------+---------+
|**Dimensions**            |[1:32]   |
+--------------------------+---------+

.. note::
       With global average pooling:
              * the output of the convolution must have at least 3 rows
              * 1x1 inputs are not supported

+----------------+---------+-----------+--------------+
|**Quantization**|**Input**|**Weights**|**Activation**|
+----------------+---------+-----------+--------------+
|**Bitwidth**    |1, 2, 4  |2, 4       |1, 2, 4       |
+----------------+---------+-----------+--------------+

FullyConnected
^^^^^^^^^^^^^^

+--------------+---------+----------+---------+
|**Input**     |**Width**|**Height**|**WxHxC**|
+--------------+---------+----------+---------+
|**Dimensions**|1        |1         |<= 57334 |
+--------------+---------+----------+---------+

+----------------+---------+-----------+--------------+
|**Quantization**|**Input**|**Weights**|**Activation**|
+----------------+---------+-----------+--------------+
|**Bitwidth**    |1, 2, 4  |1, 2, 4    |1, 2, 4       |
+----------------+---------+-----------+--------------+

