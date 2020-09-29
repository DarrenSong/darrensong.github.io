---
layout: post
title:  "ButterWorth滤波"
date:   2017-04-18 22:49:52 +0800
categories: 嵌入式开发
tags: Matlab
---
```matlab
function y=highp(x,f1,f3,rp,rs,Fs)
%高通滤波
%使用注意事项：通带或阻带的截止频率的选取范围是不能超过采样率的一半
%即，f1,f3的值都要小于 Fs/2
%x:需要带通滤波的序列
% f 1：通带截止频率
% f 2：阻带截止频率
%rp：边带区衰减DB数设置
%rs：截止区衰减DB数设置
%FS：序列x的采样频率
% rp=0.1;rs=30;%通带边衰减DB值和阻带边衰减DB值
% Fs=2000;%采样率
%频率归一化
wp=2*f1/Fs;
ws=2*f3/Fs;
%{
% 设计切比雪夫滤波器； [n,wn]=cheb1ord(wp/pi,ws/pi,rp,rs);
% [bz1,az1]=cheby1(n,rp,wp/pi,'high');
%}
%% 设计巴特沃斯滤波器   
%//注意切比雪夫和巴特沃斯传入参数的不同rp
[n,wn]=buttord(wp,ws,rp,rs);
[bz1,az1]=butter(n,wn,'high');
%{
%查看设计滤波器的曲线
% [h,w]=freqz(bz1,az1,256,Fs);  %求离散系统频响特性的函数freqz()
% h=20*log10(abs(h));
% figure;plot(w,h);title('所设计滤波器的通带曲线');grid on;
%}
%% 滤波函数 
y=filter(bz1,az1,x);
end
```