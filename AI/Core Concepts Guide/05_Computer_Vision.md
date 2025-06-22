# AI Core Concepts (Part 5): Computer Vision for Software Engineers

**Computer Vision (CV)** enables machines to understand and interpret visual data such as images and videos. It powers applications like face detection, object recognition, autonomous driving, and medical imaging.

---

## 1. Image Classification

Assign a label to an entire image (e.g., “cat”, “dog”).

**Example: Classifying images with a pretrained ResNet**
```
from torchvision import models, transforms
from PIL import Image
import torch

# Load pretrained model
model = models.resnet18(pretrained=True)
model.eval()

# Preprocessing pipeline
preprocess = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor()
])

img = Image.open("example.jpg")
input_tensor = preprocess(img).unsqueeze(0)

# Predict
with torch.no_grad():
    output = model(input_tensor)
    predicted_class = output.argmax().item()
print("Predicted class index:", predicted_class)
```

---

## 2. Object Detection

Locate and classify multiple objects in an image, outputting **bounding boxes** and labels.

**Example: Using a pretrained model for object detection**
```
from torchvision.models.detection import fasterrcnn_resnet50_fpn
import torchvision.transforms.functional as F

model = fasterrcnn_resnet50_fpn(pretrained=True)
model.eval()

img = Image.open("street.jpg")
tensor_img = F.to_tensor(img).unsqueeze(0)

with torch.no_grad():
    predictions = model(tensor_img)

for box, label, score in zip(predictions[0]['boxes'], predictions[0]['labels'], predictions[0]['scores']):
    if score > 0.8:
        print(f"Object {label} with confidence {score:.2f}, box: {box}")
```

---

## 3. Image Segmentation

Label each pixel in an image by category (e.g., “sky”, “road”, “car”). It’s useful in self-driving cars, robotics, and medical imaging.

**Example: Semantic segmentation**
```
from torchvision.models.segmentation import deeplabv3_resnet50

model = deeplabv3_resnet50(pretrained=True)
model.eval()

img = Image.open("road_scene.jpg")
tensor_img = F.to_tensor(img).unsqueeze(0)

with torch.no_grad():
    output = model(tensor_img)["out"][0]
    segmentation = output.argmax(0)
print("Segmentation map shape:", segmentation.shape)
```

---

## 4. Preprocessing Visual Data

Before feeding images into models, we must normalize, resize, and convert them to tensors.

**Example: Image preprocessing pipeline**
```
transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],
                         std=[0.229, 0.224, 0.225])
])
```

---

## 5. Common Architectures

- **CNNs (Convolutional Neural Networks)** – foundational to most CV tasks.
- **ResNet** – introduces residual connections to improve deep network training.
- **YOLO** – real-time object detection.
- **UNet** – popular in medical imaging for segmentation.
- **Vision Transformers (ViTs)** – apply self-attention to images instead of convolutions.

---

## 📚 Further Resources

- [CS231n: Convolutional Neural Networks for Visual Recognition](http://cs231n.stanford.edu/)
- [Ultralytics YOLOv8 Docs](https://docs.ultralytics.com/)
- [FastAI: Vision Course](https://course.fast.ai/)

---
