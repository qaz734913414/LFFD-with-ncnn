## the C++ implemententation of LFFD with ncnn 
  I have implemented the LFFD  referring to the official python implementation
  the Inference time of LFFD with the input shape of 320x240 is about 20ms on the Qualcomm Snapdragon 632 CPU
  
  paper:[LFFD: A Light and Fast Face Detector for Edge Devices](https://arxiv.org/abs/1904.10633)
  
  official github: [LFFD](https://github.com/YonghaoHe/A-Light-and-Fast-Face-Detector-for-Edge-Devices)
  
  My MNN implementation [MNN](https://github.com/SyGoing/LFFD-MNN).
  
  My OpenVINO [implementation](https://github.com/SyGoing/LFFD-OpenVINO)
## some tips
 * You can set the input tensor shape smaller ,since you need to reduce the memory and accelerate the inference.
 * You can set the scale_num=8 to use another larger model. 
 * I just test it on vs2019 PC and the result is correct compared to original implementation,you can use the code to another device such as android、RK3399、and so on.

## how to convert the original model to ncnn
  The original mxnet model has merged the preporcess(means and norms) and the detection output tensor has been sliced with the mxnet slice op in the symbol ,which caused convert failure.
so,you need to remove these ops ,in that way you can convert the model to onnx/ncnn successfully.I will show you how to do that step by step, so when you train the model by yourself,
you can convert to your own model to onnx , and do more things.

  * First ,follow the author's original  github to build the devolopment environment.
  
  * Modify symbol_10_320_20L_5scales_v2.py (your_path/A-Light-and-Fast-Face-Detector-for-Edge-Devices\face_detection\symbol_farm) 
  
      in function loss_branch,Note out（注释掉） the line 57(predict_score = mxnet.symbol.slice_axis(predict_score, axis=1, begin=0, end=1)
	  
	  in function get_net_symbol, Note out（注释掉）the line 99(data = (data - 127.5) / 127.5,preprocess).
	  
  * Next,in this path , by doing "python symbol_10_320_20L_5scales_v2.py	",generate the symbol.json. symbol_10_560_25L_8scales_v1.py do the same thing .
  

## TODO(you can refer this implementation to do more)
 - [x] MNN demo finished
 - [x] openvino demo: mxnet model-->onnx-->openvino
 - [x] TensorRT demo: mxnet model --> onnx-->trt engine(coming soon)

  
  
