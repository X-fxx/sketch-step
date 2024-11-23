# sketch-step
## 2024.11.22
1. ControlNet-sdxl-1.0  [https://huggingface.co/xinsir/controlnet-scribble-sdxl-1.0]
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
## 2024.11.23
diffusers中不同的模型，返回的内容不同，更换模型后需要打印输出具体的模型返回结构，得到对应的内容 \
### 问题：如何得到内部的返回结果，来进行inverse
![origin]![image](https://github.com/user-attachments/assets/2e63755d-87c8-4ca5-8603-973b2afcf04f)
1. 在实验的过程中，反演的图片应该是固定的**某一张图片**，而不是生成的图片再拿去这反演，如果这样使用的话，草图在这个过程中起不到控制作用，有点类似于对第一张图片做锐化的操作。
 ![image](https://github.com/user-attachments/assets/dd7f8aa3-6ef3-4767-8ea2-c92e36821e96)
![image](https://github.com/user-attachments/assets/638880af-4637-4395-b45d-227766489d87)
2. 对于同一张图片，反演到noise再重新生成（正向生成用30步，inverse 30步），得到的内容与之前是不同的（单纯的反演，并不能起到保证内容不变的操作）
![image](https://github.com/user-attachments/assets/664c091a-ee8b-489a-9bfd-a304acfeeace)
3. 如果只inverse一步，得到的内容与起点图片是完全相同的（整体的内容已经确定，后续只是在细节上有细微的差距）
4. ![inverstep=10]![image](https://github.com/user-attachments/assets/6960e0d3-3afe-4f43-b6f2-18d43aad3af3)
![inverse_step=20]![image](https://github.com/user-attachments/assets/d8c8a194-60c8-477d-8e7d-9b459b4bc444)





