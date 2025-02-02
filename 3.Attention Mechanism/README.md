## Coding Attention Mechanism

This chapter covers
- The reasons fo using attention mechanisms in meural networks
- A basic self-attention framework, progressing to an enhanced self-attention mechanism
- A causal attention module that allows LLMs to generate one token at a time
- Masking randomly selected attention weights with dropout to reduce overfitting
- Stacking multiple causal attention modules into a multi-head attention module


#### Problems with older Neural Network Models
1. Recurrent Neural Networks (RNNs)
- RNNs introduced the concept of memory by generating a hidden state at every step. 
- For every input token, a hidden state is generated
- For every subsequent input tokens, the previous hidden state in passed along to the encoder to generate the next subsequent hidden state
- The step is continued until the last token is processed
- The reason this is a problem because, when processing a long sequence, only one final hidden state is generated. The Decoder takes this last hidden state as the memory
- RNNs cant directly access earlier hidden state from the encoder, during the decoding phase.