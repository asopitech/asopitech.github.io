# GPU computing and LLMs

## Bottom line

Nim can use GPUs, but it lacks the ecosystem required to make modern LLM training Nim-centric. CUDA, cuBLAS, cuDNN, and libtorch are reachable through C/C++ interoperability; the missing layer is the mature training stack above them.

| Workload | Practicality |
| --- | --- |
| CUDA API calls | Possible |
| Custom GPU kernels | Possible, with substantial manual work |
| Small GPU tensor/NN experiments | Practical for experimentation |
| LLM inference control plane | A good fit |
| Production LoRA/QLoRA | Use Python/PyTorch |
| Large distributed pre-training | Not realistic |

Community bindings such as `nimcuda` can expose CUDA APIs. Arraymancer supports tensor and numerical work but is not a PyTorch-equivalent LLM stack. libtorch bindings deserve careful maturity and ABI evaluation.

Nim is well suited to preprocessing, parsers, tokenizers, dataset streaming, native extensions, and the batching/queue/CLI/HTTP layer around established inference runtimes:

```text
Nim application → C ABI → llama.cpp / ONNX Runtime / TensorRT / libtorch → GPU
```

Use Python/PyTorch for training, Rust for engine and resource-management infrastructure, and CUDA/C++ or Triton for specialized kernels. Nim is most compelling here for building data engines or GPU DSLs, not for recreating the LLM-training ecosystem.
