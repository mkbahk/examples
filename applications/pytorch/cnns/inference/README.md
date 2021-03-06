Graphcore
---

## Image classification inference on IPU using PyTorch

The following models are supported using this inference harness.

1. ResNet18-34-50-101-152
2. MobileNet
3. EfficientNet-B0..7
4. ResNext50-101

### File structure

* `run_benchmark.py` Driver script for running inference.
* `README.md` This file.
* `test_inference.py` Test cases for inference.
* `configs.yml` Contains the common inference configurations.

### How to use this demo

1) Install and activate the PopTorch environment as per cnns folder README.md, if you haven't done already.

2) Download the images using `get_images.sh`. The download script should be run from within the data directory:
    
```bash
cd ../data/
./get_images.sh
cd ../inference/
```

  This will create and populate the `images/` directory in the `data` folder with sample test images.

3) Run the graph.

```bash
python3 run_benchmark.py --data real --replicas 1  --batch-size 1 --model resnet18 --device-iteration 1
```

### Options

The program has a few command-line options:

`-h`                            Show usage information.

`--config`                      Apply the selected configuration.

`--batch-size`                  Sets the batch size for inference.

`--model`                       Select the model (from a list of supported models) for inference.

`--data`                        Choose the data mode between `real` and `synthetic`. In synthetic data mode (only for benchmarking throughput) there is no host-device I/O and random data is generated on the device.

`--pipeline-splits`             List of layers to create stages of the pipeline. Each stage runs on different IPUs. Example: `layer0 layer1/conv layer2/block3/bn` The stages are space separated.

`--replicas`                    Number of IPU replicas.

`--device-iterations`           Sets the device iteration: the number of inference steps before program control is returned to the host.

`--precision`                   Precision of Ops(weights/activations/gradients) and Master data types: `32.32` or `16.16`.

`--iterations`                  Sets the number of program iterations to execute.

`--half-partial`                Flag for accumulating matrix multiplication partials in half precision

`--norm-type`                   Select the used normlayer from the following list: `batch`, `group`, `none`

`--norm-num-groups`             If group normalization is used, the number of groups can be set here.

### Model configurations

Inference on single IPU.

|Model   | MK1 IPU | MK2 IPU |
|-------|----------|---------|
|ResNet50| `python3 run_benchmark.py --config resnet50-mk1` | `python3 run_benchmark.py --config resnet50-mk2`  |
|EfficientNet-B0| `python3 run_benchmark.py --config efficientnet-b0-mk1`  |`python3 run_benchmark.py --config efficientnet-b0-mk2`|
