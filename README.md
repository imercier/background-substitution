# video conference background substitution
How to mask your background in conferences with blur or any image.  
Works with any video conference tools.  
Without green screen panel.

-----------------

### 1/ Setup
```
sudo apt-get install -q python3 python3-pip v4l2loopback-dkms
pip3 install tf_bodypix[all] tensorflow
sudo modprobe v4l2loopback devices=1 video_nr=20 card_label="v4l2loopback" exclusive_caps=1
```

#### permanent config: load v4l2loopback at each boot
```
sudo sh -c "echo v4l2loopback > /etc/modules-load.d/v4l2loopback.conf"
sudo sh -c "echo 'options v4l2loopback devices=1 video_nr=20 card_label=\"v4l2loopback\" exclusive_caps=1' > /etc/modprobe.d/v4l2loopback.conf"
```


### 2/ Choose or download background image and place it to ```~/tmp/background.jpg``` for example
https://www.gettyimages.fr/  
https://unsplash.com/collections/1887152/zoom-backgrounds

-----------------

### 3/ Runtime
#### background replacement
```
python3 -m tf_bodypix replace-background --background ~/tmp/background.jpg \
  --source webcam:0 --output /dev/video20 --source-width=1280 --source-height=720 \
  --threshold=0.9 \
  --model-path "https://storage.googleapis.com/tfjs-models/savedmodel/bodypix/mobilenet/float/075/model-stride16.json"
```

#### blur background
```
python3 -m tf_bodypix blur-background \
  --source webcam:0 --output /dev/video20 \
  --source-width=1280 --source-height=720 \
  --threshold=0.9

```

-----------------

### credit
https://github.com/tensorflow/tfjs-models/tree/master/body-pix  
https://github.com/de-code/python-tf-bodypix
