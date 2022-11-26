# ONNX Runtime Inference

## Introduction

ONNX Runtime C++ inference example for image classification using CPU and CUDA.

## Dependencies

* CMake 3.20.1
* ONNX Runtime 1.12.0
* OpenCV 4.5.2

## Usages

### Setup
```bash
$ git config --global http.postBuffer 1048576000
$ sudo pmset -a disablesleep 1  # 0 if one want to reset to normal
```


### Build Docker Image

```bash
# original script: onnxruntime-cuda.Dockerfile
$ docker build -f docker/onnxruntime-cuda.Dockerfile --no-cache --tag=onnxruntime-cuda:1.12.0 .

# onnxruntime-cpu-arm64.Dockerfile
$ docker build -f docker/onnxruntime-cpu-arm64.Dockerfile --no-cache --tag=onnxruntime-cpu-arm64:1.12.0 .

# onnxruntime-cuda-amd64onarm64.Dockerfile
$ DOCKER_DEFAULT_PLATFORM=linux/amd64 DOCKER_BUILDKIT=0 docker build -f docker/onnxruntime-cuda-amd64onarm64.Dockerfile --no-cache --platform linux/amd64 --tag=onnxruntime-cuda-amd64onarm64:1.12.0 .
```

### Run Docker Container

```bash
# original script: onnxruntime-cuda.Dockerfile
$ docker run -it --rm --gpus device=0 -v $(pwd):/mnt onnxruntime-cuda:1.12.0

# onnxruntime-cpu-arm64.Dockerfile
$ docker run -it --rm -v $(pwd):/mnt onnxruntime-cpu-arm64:1.12.0

# onnxruntime-cuda-amd64onarm64.Dockerfile
$ docker run -it --rm --gpus device=0 -v $(pwd):/mnt onnxruntime-cuda-amd64onarm64:1.12.0
```

### Build Example

```bash
$ cd mnt/
$ cmake -B build
$ cmake --build build --config Release --parallel
```

### Run Example

```bash
$ cd build/src/
$ ./inference  --use_cpu
Inference Execution Provider: CPU
Number of Input Nodes: 1
Number of Output Nodes: 1
Input Name: data
Input Type: float
Input Dimensions: [1, 3, 224, 224]
Output Name: squeezenet0_flatten0_reshape0
Output Type: float
Output Dimensions: [1, 1000]
Predicted Label ID: 92
Predicted Label: n01828970 bee eater
Uncalibrated Confidence: 0.996137
Minimum Inference Latency: 7.45 ms
```

```bash
$ cd build/src/
$ ./inference  --use_cuda
Inference Execution Provider: CUDA
Number of Input Nodes: 1
Number of Output Nodes: 1
Input Name: data
Input Type: float
Input Dimensions: [1, 3, 224, 224]
Output Name: squeezenet0_flatten0_reshape0
Output Type: float
Output Dimensions: [1, 1000]
Predicted Label ID: 92
Predicted Label: n01828970 bee eater
Uncalibrated Confidence: 0.996137
Minimum Inference Latency: 0.98 ms
```

## References

* [ONNX Runtime C++ Inference](https://leimao.github.io/blog/ONNX-Runtime-CPP-Inference/)
