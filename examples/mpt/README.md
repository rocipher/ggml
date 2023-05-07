# MPT-7B 

**NOTE: This implementation attempt DOES NOT WORK (outputs gibberish during inference), most probably because it does not include
the required port of the [modeling_mpt.py](https://github.com/mosaicml/llm-foundry/blob/3e16277b73e1cadb8409f6cd62dd7ea9610d0311/llmfoundry/models/mpt/modeling_mpt.py)
from MosaicML's repository.**

The GGML conversion and quantization parts seem to work.

Transformer architecture: MPT (?)

Modeled from examples/stablelm

Ref: https://huggingface.co/mosaicml/mpt-7b

Ref: https://github.com/stability-AI/stableLM/#stablelm-alpha

## Usage

```bash
# get the repo and build it
git clone https://github.com/ggerganov/ggml
cd ggml
mkdir build && cd build
cmake ..
make -j

# get the MPT-7B model
git clone https://huggingface.co/mosaicml/mpt-7b

# convert model to FP16
python3 ../examples/mpt/convert-h5-to-ggml.py ./mpt-7b/ 1

# run inference using FP16 precision
./bin/mpt -m ./mpt-7b/ggml-model-f16.bin -p "State the meaning of life." -t 6 -n 64

...
```

## 5-bit integer quantization mode

```bash
# quantize the model to 5-bits using Q5_1 quantization
./bin/mpt-quantize ./mpt-7b/ggml-model-f16.bin ./mpt-7b/ggml-model-q5_1.bin 9

# run the quantized model
./bin/mpt -m ./mpt-7b/ggml-model-q5_1.bin -p "State the meaning of life." -t 6 -n 64

...
```

## Notes

- No guarantees for correctness
- The tokenizer is currently hacked - probably works only for English
- Non-parallel residual is not supported
- Contributions and improvements are welcome

## Note about possible bugs

**This implementation attempt does not work, it does not include the required port of modeling_mpt.py .**
