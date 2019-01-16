# LAB1 - Prepare Environment

## <Step 0 - Install> 
https://software.intel.com/en-us/articles/OpenVINO-Install-Linux

## <Step 1 - Run demo script>
```
source /opt/intel/computer_vision_sdk/bin/setupvars.sh
cd /opt/intel/computer_vision_sdk/deployment_tools/demo/
./demo_squeezenet_download_convert_run.sh
```

This script will 
1. Download/ install needed packages 
2. Download squeeznet into $home/openvino_models folder
3. run model optimizer to compile caffe model into intel IR format
4. Compile and Run and classification sample for card

Result - 
[out put result - click the link to show image](images/01.png)
 
## <Step 2 - Following Steps>
After exection, there’s two folder appear in $HOME. (1) Inference_engine_simpile (2) openvino_models
Please use other images to run ‘classification_simple’ 

# LAB2 – Run Intel Pretrained models
## <Step1 build Sample Code>
```
mkdir ~/build/
cd ~/build/
cmake /opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples/
make
```

## <Step2 – check the result>
```
ls ~/build/intel64/build/
```
[out put result - click the link to show image](images\02.png)
 

## <Step3 execute a command>
```
cd ~/build/intel64/Release/
./object_detection_demo_ssd_async -d CPU -m /opt/intel/computer_vision_sdk/deployment_tools/intel_models/face-detection-retail-0004/FP32/face-detection-retail-0004.xml -i ~/Movie/face.mp4
./interactive_face_detection_demo -d CPU -m /opt/intel/computer_vision_sdk/deployment_tools/intel_models/face-detection-retail-0004/FP32/face-detection-retail-0004.xml -i ~/Movie/face.mp4
./interactive_face_detection_demo -d CPU -m /opt/intel/computer_vision_sdk/deployment_tools/intel_models/face-detection-retail-0004/FP32/face-detection-retail-0004.xml -m_ag /opt/intel/computer_vision_sdk/deployment_tools/intel_models/age-gender-recognition-retail-0013/FP32/age-gender-recognition-retail-0013.xml -m_hp /opt/intel/computer_vision_sdk/deployment_tools//intel_models/head-pose-estimation-adas-0001/FP32/head-pose-estimation-adas-0001.xml -m_em /opt/intel/computer_vision_sdk/deployment_tools/intel_models/emotions-recognition-retail-0003/FP32/emotions-recognition-retail-0003.xml -i ~/Movie/face.mp4
```

## <step4 Following Study>
Goto the online doc - https://software.intel.com/en-us/articles/OpenVINO-IE-Samples and check how to use IE sample code

# LAB3 – Run a real Example

## <step 1 choose a model > 
Please check OpenVINO online documents https://software.intel.com/en-us/articles/OpenVINO-ModelOptimizer or use the model from tensorflow/ caffe or the model which you’re using now.

## <step 2 Convert by MO / example commands for MO>
```
cd /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/install_prerequisites/ && ./install_prerequisites_tf.sh && cd ~
wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2018_01_28.tar.gz
tar zxvf ssd_mobilenet_v1_coco_2018_01_28.tar.gz && cd ssd_mobilenet_v1_coco_2018_01_28
/opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/mo_tf.py --input_model=./frozen_inference_graph.pb --tensorflow_use_custom_operations_config /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json --tensorflow_object_detection_api_pipeline_config ./pipeline.config --reverse_input_channels
```
[out put result - click the link to show image](images\03.png)

## <step 2 Run IE / example commands for IE>
```
~/build/intel64/Release/object_detection_demo_ssd_async -d CPU -m ./frozen_inference_graph.xml -i ~/Movie/face.mp4 
```


# LAB4 – Run it on FPGA
Use the server in the classroom the test the model and commands in Lab1/2/3 with hetero plugin

