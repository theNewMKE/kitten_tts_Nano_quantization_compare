😻 Kitten TTS Model Quantization Comparison FP32 & INT8

Cosine similarity measures the cosine of the angle between two vectors projected in a multi-dimensional space. 
It determines how similar the two vectors are in terms of their direction, \regardless of their **magnitude.**

For two flattened weight tensors (or activation tensors), $A$ (FP32) and $B$ (INT8), the formula is:

$$\text{Cosine Similarity} = \frac{A \cdot B}{\|A\|\|B\|} = \frac{\sum_{i=1}^{n} A_i B_i}{\sqrt{\sum_{i=1}^{n} A_i^2} \sqrt{\sum_{i=1}^{n} B_i^2}}$$


The result ranges from **-1 to 1:**


*   1.0: Perfect alignment (identical directions)
*   0.0: Orthogonal (completely uncorrelated).
*   -1.0: Completely opposed (worst).

In quantization, you generally want scores above 0.99 for weights.

 Anything lower usually indicates a layer that is highly sensitive to quantization and might need to be kept in FP16 or FP32.
 
When you calculate the cosine similarity, you cannot directly compare raw INT8 integers to FP32 floats because cosine similarity is sensitive to shifts (the zero-point). 

Before calculating the similarity, you must simulate the hardware's process and dequantize the INT8 weights back into a dequantized FP32 format using their specific scale and zero-point parameters:

$$\text{Weight}_{\text{FP32}} = (\text{Weight}_{\text{INT8}} - \text{ZeroPoint}) \times \text{Scale}$$

