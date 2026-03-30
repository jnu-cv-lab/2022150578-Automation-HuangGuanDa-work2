实验一：C++ 视觉开发环境搭建与图像基本读写
实验信息

实验课程：计算机视觉实验
实验名称：环境搭建与图像基本读写
实验日期：2026年3月30日

一、实验目的

搭建 C++ OpenCV 视觉开发环境（VS Code + WSL + Ubuntu）
掌握 OpenCV 在 C++ 中的基本图像操作（读取、显示、转换、保存）
学习输出图像的基本信息（尺寸、通道数、数据类型）
使用 OpenCV Mat 和 NumPy 风格进行简单的像素访问和图像裁剪操作

二、实验环境

操作系统：Windows 11 + WSL (Ubuntu 22.04)
编程语言：C++17
主要库：
OpenCV 4.6.0
NumPy 风格操作（通过 OpenCV Mat 实现）

开发工具：Visual Studio Code + C/C++ 扩展
构建方式：g++ + pkg-config

三、完整代码（main.cpp）
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

int main() {
    // 任务1：使用 OpenCV 读取测试图片
    Mat img = imread("test.jpg", IMREAD_COLOR);
    
    if (img.empty()) {
        cout << "错误：找不到图片文件！" << endl;
        return -1;
    }

    // 任务2：输出图像基本信息
    cout << "图像尺寸: " << img.cols << " x " << img.rows << endl;
    cout << "通道数: " << img.channels() << endl;
    cout << "数据类型: " << img.type() << " (CV_8UC3 = 16)" << endl;

    // 任务3：显示原图
    namedWindow("原图", WINDOW_AUTOSIZE);
    imshow("原图", img);

    // 任务4：转换为灰度图并显示
    Mat gray;
    cvtColor(img, gray, COLOR_BGR2GRAY);
    namedWindow("灰度图", WINDOW_AUTOSIZE);
    imshow("灰度图", gray);

    // 任务5：保存灰度图
    imwrite("gray_test.jpg", gray);
    cout << "灰度图已保存" << endl;

    // 任务6：NumPy风格操作 - 输出左上角像素值
    if (!img.empty()) {
        Vec3b pixel = img.at<Vec3b>(0, 0);
        cout << "左上角像素值 (B, G, R): [" 
             << (int)pixel[0] << " " 
             << (int)pixel[1] << " " 
             << (int)pixel[2] << "]" << endl;
    }

    // 任务6：NumPy风格操作 - 裁剪左上角区域并保存
    int crop_size = 300;
    if (img.rows >= crop_size && img.cols >= crop_size) {
        Mat cropped = img(Rect(0, 0, crop_size, crop_size));
        imwrite("cropped.jpg", cropped);
        cout << "裁剪区域已保存 (" << crop_size << "x" << crop_size << ")" << endl;
        
        namedWindow("裁剪区域", WINDOW_AUTOSIZE);
        imshow("裁剪区域", cropped);
    }

    // 等待按键后关闭所有窗口
    cout << "\n按任意键关闭窗口..." << endl;
    waitKey(0);
    destroyAllWindows();

    return 0;
}

四、功能说明
任务1读取测试图片imread("test.jpg", IMREAD_COLOR)
任务2输出图像尺寸、通道数、数据类型img.cols, img.rows, img.channels(), img.type()
任务3显示原图imshow("原图", img)
任务4转换为灰度图并显示cvtColor(img, gray, COLOR_BGR2GRAY)
任务5保存灰度图imwrite("gray_test.jpg", gray)
任务6输出左上角像素值img.at<Vec3b>(0, 0)
任务7裁剪左上角区域并保存img(Rect(0, 0, 300, 300))

五、 显示效果
程序会弹出三个图像窗口：
原图：显示原始彩色图片
灰度图：显示转换后的灰度图片
裁剪区域：显示裁剪后的左上角区域（可选）

六、实验总结
本次实验成功完成了以下内容：
✅ 搭建了 C++ OpenCV 视觉开发环境（VS Code + WSL + g++）
✅ 掌握了 OpenCV 在 C++ 中的基本图像操作
✅ 学会了获取图像的基本信息（尺寸、通道数、数据类型）
✅ 实现了图像的灰度转换、显示与保存
✅ 使用 OpenCV Mat 进行了 NumPy 风格的图像裁剪和像素访问操作
