
[CUDA Application Compatibility Support Matrix](https://docs.nvidia.com/deploy/cuda-compatibility/index.html#use-the-right-compat-package)

[PyTorch with CUDA support](https://stackoverflow.com/a/62361395/20015297)

## Tips

- `nvidia-smi -lms 500` will loop and call the view at every 500 milliseconds. Alternatively, `watch -n 0.5 nvidia-smi`, where 0.5 is the time interval in seconds, avoids filling terminal with output.