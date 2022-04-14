## Deepstream notes: 

[deepstream-test1](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/blob/master/apps/deepstream-test1/deepstream_test_1.py)

The pipeline is as follow:
Source (file) 
- -> h264parser 
- -> decoder (nvv4l2-decoder) 
- -> nvstreammux (streamuxer) 
- -> pgie (primary inference) 
- -> nvdosd (on screen display)
- -> nvegl-transform (ONLY for Jetson)
- -> nveglglessink (output for display)

## step 1: You define each step of the pipeline
## step 2: You set the properties of given steps:
 - ex: the streammuxer has `height width batch_size batched-push-timeout` properties
 - ex: the primary predictor has `config-file-path` property
 
## step 2: You link each step of the pipeline
## step 3: You define an event loop where Gstreamer send data
    ```
    loop = GObject.MainLoop()
    bus = pipeline.get_bus()
    bus.add_signal_watch()
    bus.connect ("message", bus_call, loop)
    ```
    
## step 4: You can add "probes to fetch information (bounding boxes coordinates for ex)
ex: you can set a probe on the on screen display step by using a *static pad sink*

` osdsinkpad = nvosd.get_static_pad("sink") `
` osdsinkpad.add_probe(Gst.PadProbeType.BUFFER, osd_sink_pad_buffer_probe, 0) `

## step5: run the pipeline with an infinite loop
## step6: set the state of the pipeline to void/null
`pipeline.set_state(Gst.State.PLAYING)`
 
