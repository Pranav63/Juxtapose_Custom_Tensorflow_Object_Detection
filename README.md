# Juxtapose_Custom_Tensorflow_Object_Detection
To extend the extent of detection by Tensor Flow object detection APIs and Custom Detector built using apposite techniques and tools. 
 # PROCEDURE TO RUN:- 
First, install the scripts requirements:  pip install -r requirements.txt 
Compile the Protobuf libraries:  protoc object_detection/protos/*.proto --python_out=.  
Add models and models/slim to your PYTHONPATH:  export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim 
1) Create the TensorFlow Records Run the script:  python object_detection/create_tf_record.py 

2) Download a Base Model  Models to download from this model zoo. Use faster_rcnn_resnet101_coco.  Extract the files and move all the model.ckpt to our models directory. 

3) Train the Model Run the following script to train the model:  python object_detection/train.py \ --logtostderr \ --train_dir=train \ --pipeline_config_path=faster_rcnn_resnet101.config  

4) Export the Inference Graph  When you model is ready depends on your training data, the more data, the more steps you’ll need. My model was pretty solid at ~2.5k steps.  
Move the model.ckpt files with the highest number to the root of the repo into a frozen inference graph by running this command:  python object_detection/export_inference_graph.py \  --input_type image_tensor \  --pipeline_config_path faster_rcnn_resnet101.config \  --trained_checkpoint_prefix model.ckpt-STEP_NUMBER \  --output_directory output_inference_graph You should see a new output_inference_graph directory with a frozen_inference_graph.pb file. 

5) Test the Model  python object_detection/object_detection_runner.py It will run your object detection model found at output_inference_graph/frozen_inference_graph.pb on all the images in the test_images directory and output the results in the output/test_images directory.
