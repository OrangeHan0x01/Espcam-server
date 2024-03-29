项目说明：

分为服务器部分和硬件部分，硬件端基于esp32cam和nrf24l01，服务器软件基于python+flask。
主要目的是实现从esp32cam获取图像并进行存储/实时显示，也支持使用2.4G无线电进行中距离传输（但速率会较低，尚未进行优化，只是草草实现了功能）。

服务器支持如下功能：

![image](https://github.com/OrangeHan0x01/Espcam_server/blob/main/how_to.png)

一、抓取图像：
1、从视频流页面抓取图像，并根据目标所在ip确定是哪个摄像头；
2、从tcp-socket接收图像，并根据图像字节流前几个字节与摄像头列表中每个摄像头序号的最后几个字节进行对比，确认属于哪个摄像头；

二、图像后续处理：
1、支持jpeg字节流转图像，图像大小变换、存储、多张图片合并为一张图片、合成视频，理论上可以同时获取多个摄像头数据合并存储但并未测试。
2、支持网页实时显示接收到的图片流。

硬件三个文件支持如下功能：
1、comp_tcp：获取图像并通过tcp连接传输到服务器
2、nrf_client：获取图像并通过NRF24L01传输到nrf_server端；
3、nrf_server:从nrf_client获取图像并通过tcp传输到服务器；（只支持一对一传输，还没有进行任何优化，大约0.5-0.33fps），images下的图像是用这个功能获取的

环境说明：
服务器环境：需要安装python，库：flask，numpy,socket，cv2，版本应该没影响所以不写requirements了

硬件环境：Arduino IDE + esp32cam环境
需要的RF24库为 https://github.com/cyberarts/RF24

esp32cam连线注意：连线方式在nrf_client和nrf_server中，如果自带led是亮的说明连线错误。
用烧录座烧录时，在编译完成前同时按住boot和reset，到上传中时一起松开。

使用说明：
PC服务器:app.py，注意这个服务器是用于tcp传输功能的，如果需要从视频流页面抓取图像的方式，需要进行修改。


