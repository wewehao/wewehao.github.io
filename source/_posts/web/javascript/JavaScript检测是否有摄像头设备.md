---
title: JavaScript检测是否有摄像头设备
date: 2021-11-17 16:32:19
tags:
---

## 通过浏览器自带的API实现

```javascript
// 获取设备列表
async getDeviceList() {
    return new Promise(resolve => {
        navigator.mediaDevices.enumerateDevices().then(devices => {
            let audioDevices = [];
            let videoDevices = [];
            for (let i = 0; i < devices.length; i++) {
                if (devices[i].kind === 'audioinput') {
                    audioDevices.push(devices[i]);
                } else if (devices[i].kind === 'videoinput') {
                    videoDevices.push(devices[i]);
                }
            }
            resolve({ audio: audioDevices, video: videoDevices });
        }).catch((err) => {
            console.log(err);
            resolve({ audio: [], video: [] });
        });
    });
}
```