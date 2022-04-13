# deepstreamer

## 1. Which of the following statements are true about "bounding boxes" in the context of object detection? (Check all that apply).

- Bounding boxes are used to show target locations.

- A bounding box is a rectangular box determined with x and y coordinates of the axis.

- A bounding box is used to shrink the size of the overall image.

- In a DeepStream pipeline, the Gst-nvdsosd plugin is used to draw bounding boxes, text, and region-of-interest (RoI) polygons.


## 2. What is the Gst-nvinfer plugin used for? (Check all that apply)

- Performs transforms (format conversion and scaling), on the input frame based on network requirements, and passes the transformed data to the low-level library

- Performs inferencing on input data using NVIDIA TensorRT

- Sends UDP packets to the network.

- Tracks object between frames.


## 3. What feature describes a "hardware-accelerated" plugin?

- Interacts with hardware to deliver maximum performance such as GPU, DLA, PVA

- Improves High level software interactions such as Python or Java

- Executes on embedded processors

## 4. Looking at the config file on your Jetson Nano at: 
"/home/<user>/deepstream_sdk_v4.0.2_jetson/sources/apps/dli_apps/deepstream-test1-rtsp_out/dstest1_pgie_config.txt”: 
    
 What can we understand about the model we are using for object detection and counting? 
    (Hint: check out the "model-file", "num-detected-classes", and "output-blob-names" keys)

- ResNet-10, Number of Classes 4, Output Layer: conv2d_bbox

- AlexNet, Number of Classes 100, Output Layer: conv2d_bbox

- ResNet-10, Number of Classes 100, Output Layer: conv2d_bbox

- AlexNet, Number of Classes 1000, Output Layer: conv2d_bbox

## 5. Looking at the "C" file at :
`/home/<user>/deepstream_sdk_v4.0.2_jetson/sources/apps/dli_apps/deepstream-test1-rtsp_out/deepstream_test1_app.c`: 
In line 67, we use the “NvDsBatchMeta”, metadata structure. Why is it needed? (Check all that apply)

- It is not actually used.

- We need a metadata structure to hold frames, object, classifier, and label data.

- We need to access the metadata in this structure to determine how many objects are in each frame and display them

- It is a piece of legacy code carried forward from a previous version of DeepStream.
    

## 6. What type(s) of network(s) are supported by Gst-nvinfer?

- Multi-class object detection only

- Multi-label classification only

- Multi-class object detection, multi-label classification, and segmentation

## 7. What is Gst-nvstreammux used for? (Check all that apply)

- It forms a batch of frames from one or multiple input sources

- It collects an average of (batch-size/num-source) frames per batch from each source

- It runs inference

- It tracks objects between frames

## 8. How should we determine the batch size for multiple stream inputs?

- It should be equal to or proportional to the number of input streams

- Batch size is inversely proportional to number of streams

- Batch size can be 1 even for multiple stream inputs


## 9. Which of the following plugin lists are included in both:
    - Notebook #2: "Multiple Networks Application" 
    - Notebook #3: "Multiple Stream Inputs"?

- Gst-nvinfer (for deep learning inference), Gst-nvstreammux (for batching video streams), Gst-nvtracker (tracks object between frames)

- Gst-nvinfer (for deep learning inference), Gst-nvdsosd (for drawing bounding boxes), Gst-nvvideoconvert (for video format conversion)

- Gst-nvinfer (for deep learning inference), Gst-nvmultistreamtiler (composites a 2D tile from batched buffers), Gst-nvtracker (tracks object between frames)

- Gst-nvvideoconvert (for video format conversion), Gst-nvstreammux (for batching video streams), Gst-nvmultistreamtiler (composites a 2D tile from batched buffers)
    

## 10. What are the DeepStream supported object detection networks? (Check all that apply)

-ResNet-10

- YOLO

- SSD 

- Faster-RCNN

## 11. What can be understood by looking at the config file:
 > `/home/<user>/deepstream_sdk_v4.0.2_jetson/sources/apps/dli_apps/deepstream-test3-mp4_out-yolo/dstest3_pgie_config_yolov3_tiny.txt` ?

- Neural Network Model: YOLO-V3, Number of Classes: 4, Network Mode = FP32 (Floating Point Computations), Input to model in format: BGR

- Neural Network Model: Tiny-YOLO-V3, Number of Classes: 80, Network Mode = FP32 (Floating Point Computations), Input to model in format: RGB

- Neural Network Model: Tiny-YOLO-V3, Number of Classes: 80, Network Mode = INT8 (Floating Point Computations), Input to model in format: BGR

## 12. Which of the following DeepStream plugins are part of the :
    > Primary Detector -> Object Tracker -> Secondary Classifier(s) 
    sequence used in the pipeline for Multiple Network Applications in Notebook #2? (Check all that apply):

- Gst-nvinfer

- Gst-nvvideoconvert

- Gst-nvmultistreamtiler

- Gst-nvtracker
    

## 13. What kind of video container (file type) can a DeepStream video output be saved to? (Check all that apply): 

- mp4

- avi

- anything GStreamer supports

## 14. Which of the following statements are true about DeepStream SDK? (Check all that apply)

- DeepStream SDK is based on the GStreamer framework

- DeepStream SDK is not designed to optimize performance

- DeepStream SDK is supported on systems that contain NVIDIA Jetson modules, and NVIDIA dGPU adapters.

- DeepStream SDK has a plugin interface for TensorRT for inferencing deep learning networks.


## 15. Which of the following are possible use cases of DeepStream? (Check all that apply)

- Intelligent Video Analytics

- AI-based video and image understanding

- Multi-sensor processing 

- Cloud-based offline processing
