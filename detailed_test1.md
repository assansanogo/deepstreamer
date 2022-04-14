## Deepstream notes: 

[deepstream-test1](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/blob/master/apps/deepstream-test1/deepstream_test_1.py)

The pipeline is as follow:
````
Source (file) 
-> h264parser 
-> decoder (nvv4l2-decoder) 
-> nvstreammux (streamuxer) 
-> pgie (primary inference) 
-> nvdosd (on screen display)
-> nvegl-transform (*ONLY* for Jetson)
-> nveglglessink (output for display)
````


## step 1: Define the pipeline
(define the pipeline + all steps)

````
print("Creating Pipeline \n ")
pipeline = Gst.Pipeline()

if not pipeline:
    sys.stderr.write(" Unable to create Pipeline \n")

# Source element for reading from the file
print("Creating Source \n ")
source = Gst.ElementFactory.make("filesrc", "file-source")
if not source:
    sys.stderr.write(" Unable to create Source \n")

# Since the data format in the input file is elementary h264 stream,
# we need a h264parser
print("Creating H264Parser \n")
h264parser = Gst.ElementFactory.make("h264parse", "h264-parser")
if not h264parser:
    sys.stderr.write(" Unable to create h264 parser \n")

# Use nvdec_h264 for hardware accelerated decode on GPU
print("Creating Decoder \n")
decoder = Gst.ElementFactory.make("nvv4l2decoder", "nvv4l2-decoder")
if not decoder:
    sys.stderr.write(" Unable to create Nvv4l2 Decoder \n")

# Create nvstreammux instance to form batches from one or more sources.
streammux = Gst.ElementFactory.make("nvstreammux", "Stream-muxer")
if not streammux:
    sys.stderr.write(" Unable to create NvStreamMux \n")

# Use nvinfer to run inferencing on decoder's output,
# behaviour of inferencing is set through config file
pgie = Gst.ElementFactory.make("nvinfer", "primary-inference")
if not pgie:
    sys.stderr.write(" Unable to create pgie \n")

# Use convertor to convert from NV12 to RGBA as required by nvosd
nvvidconv = Gst.ElementFactory.make("nvvideoconvert", "convertor")
if not nvvidconv:
    sys.stderr.write(" Unable to create nvvidconv \n")

# Create OSD to draw on the converted RGBA buffer
nvosd = Gst.ElementFactory.make("nvdsosd", "onscreendisplay")

if not nvosd:
    sys.stderr.write(" Unable to create nvosd \n")

# Finally render the osd output
if is_aarch64():
    transform = Gst.ElementFactory.make("nvegltransform", "nvegl-transform")

print("Creating EGLSink \n")
sink = Gst.ElementFactory.make("nveglglessink", "nvvideo-renderer")
if not sink:
    sys.stderr.write(" Unable to create egl sink \n")
````

## step 2: Set the properties of given steps:
````
 - ex: the streammuxer has `height` `width` `batch_size` `batched-push-timeout` properties
 :warning: the muxer "stiches the images together"
 :warning: the batch size should be set to a multiple of the number of sources
 
 - ex: the primary predictor has `config-file-path` property
````
## step 3: Add each step of the pipeline

`````
print("Adding elements to Pipeline \n")
 pipeline.add(source)
 pipeline.add(h264parser)
 pipeline.add(decoder)
 pipeline.add(streammux)
 pipeline.add(pgie)
 pipeline.add(nvvidconv)
 pipeline.add(nvosd)
 pipeline.add(sink)
 if is_aarch64():
     pipeline.add(transform)
`````


## step 4: Link each step of the pipeline 

`````
# we link the elements together
# file-source -> h264-parser -> nvh264-decoder ->
# nvinfer -> nvvidconv -> nvosd -> video-renderer
print("Linking elements in the Pipeline \n")
source.link(h264parser)
h264parser.link(decoder)

sinkpad = streammux.get_request_pad("sink_0")
if not sinkpad:
    sys.stderr.write(" Unable to get the sink pad of streammux \n")
srcpad = decoder.get_static_pad("src")
if not srcpad:
    sys.stderr.write(" Unable to get source pad of decoder \n")
srcpad.link(sinkpad)
streammux.link(pgie)
pgie.link(nvvidconv)
nvvidconv.link(nvosd)
if is_aarch64():
    nvosd.link(transform)
    transform.link(sink)
else:
    nvosd.link(sink)
`````

## step 4: Define an event loop where Gstreamer send data
    ````
    loop = GObject.MainLoop()
    bus = pipeline.get_bus()
    bus.add_signal_watch()
    bus.connect ("message", bus_call, loop)
    ````
    
## step 5: Add "probes to fetch information (bounding boxes coordinates for ex)
ex: you can set a probe on the on screen display step by using a *static pad sink*

` osdsinkpad = nvosd.get_static_pad("sink") `
` osdsinkpad.add_probe(Gst.PadProbeType.BUFFER, osd_sink_pad_buffer_probe, 0) `

## step 6: Run the pipeline with an infinite loop
`````

 # start play back and listen to events
    print("Starting pipeline \n")
    pipeline.set_state(Gst.State.PLAYING)
    try:
        loop.run()
    except:
        pass

`````

## step 7: Set the state of the pipeline to void/null
`pipeline.set_state(Gst.State.PLAYING)`
 
