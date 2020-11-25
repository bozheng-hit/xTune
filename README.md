# stabletune
## Environment

DockerFile: `dancingsoul/pytorch:1.6.0-cuda10.1-cudnn7-devel-apex`

Install the fine-tuning code: `pip install ./`

## Fine-tuning Command

Currently only XLM-Roberta is supported.
Our experiments is conducted on 32GB V100. 
Reduce `per_gpu_train_batch_size` and increase `gradient_accumulation_steps` if needed.

The complete stabletune is consisted of a two-stage training process.



