---
title: WebVTT字幕文件格式化
date: 2022-01-15 21:00:00
tags:
---

## 需求场景

#### 在用户端上传了视频后，服务端异步生成字幕文件，返回给前端一个`.vtt`格式的文件链接，前端需要解析到页面上，并且可以进行`编辑`。

#### 如果是仅用在视频播放器上，通常播放器会封装好字幕相关的资源，支持直接解析`.vtt`文件资源。但是需求是需要可以`编辑`，这里就需要前端自己进行解析了，或者后端解析成JSON格式给前端。

#### WebVTT文件内容通常如下

`
WEBVTT

0
00:00:00.000 --> 00:00:20.000
DPlayer 字幕： 四月是你的谎言OP - 若能绽放光芒 / Goose house

1
00:00:20.875 --> 00:00:31.800
がりの虹も 凛と咲いた花も 色づき溢れ出す

2
00:00:32.875 --> 00:00:44.175
茜色の空 仰ぐ君に  あの日 恋に落ちた

3
00:00:44.425 --> 00:00:50.025
瞬间のドラマチック フィルムの中の1コマも

4
00:00:50.450 --> 00:00:59.100
消えないよ 心に刻むから

5
00:00:59.275 --> 00:01:04.750
君だよ 君なんだよ 教えてくれた
`

#### 解析VTT文件代码如下

```javascript
export async function parseWebVTT(vttUrl) {
  return new Promise((resolve) => {
    window.fetch(vttUrl, {
      headers: {
        'Content-Type': 'text/plain;charset=utf-8'
      }
    }).then((response) => response.text()).then((texts) => {
      try {
        let subtitles = [];
        texts = texts.split('\r\n\r\n');
        texts = texts.filter((v) => v && v.includes('\r\n') && v.includes(' --> '));
        texts.forEach((v) => {
          const [i, times, ...args] = v.split('\r\n');
          const index = Number(i);
          const [startTime, endTime] = times.split(' --> ');
          subtitles[index] = {
            index,
            startTime,
            endTime,
            text: args.join('\r\n'),
          };
        });
        resolve(subtitles);
      } catch (err) {
        console.log(err);
        resolve([]);
      }
    }).catch((err) => {
      console.log(err);
      resolve([]);
    });
  });
}
```
