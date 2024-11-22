# sketch-step
## 2024.11.22
1. ControlNet-sdxl-1.0  [huggingface]https://huggingface.co/xinsir/controlnet-scribble-sdxl-1.0
2. 两种获得轮廓的方式(输入的图片是二值形式的，只有0/255两个取值，并且**轮廓的边缘是白色，背景是黑色**)
   - HEG 提取边缘
   - 直接使用sketch
   - HEG边缘与sketch对比 ![image](https://github.com/user-attachments/assets/06a9e9e8-ef13-46dc-a497-be8eabd9f5c5)
```
# python
# sketch处理方式（轮廓是黑色，背景是白色）
controlnet_img = np.array(controlnet_img)
controlnet_img[controlnet_img > 10] = 255
controlnet_img[controlnet_img < 255] = 0
controlnet_img = 255 - controlnet_img   
controlnet_img = Image.fromarray(controlnet_img)
```
3. SDXL中ControlNet最大的controlnet_conditioning_scale为1，通常最好是取0.5。
![image](https://github.com/user-attachments/assets/03ab7029-2fd8-4b17-8fac-457a9f0fe3f8)


