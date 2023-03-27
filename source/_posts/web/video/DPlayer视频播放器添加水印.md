---
title: DPlayer视频播放器添加水印
date: 2022-03-18 21:32:01
tags:
---

![](/images/dplayer_1.jpeg)

## 代码实现

> 简易的利用canvas实现铺满全屏的水印背景

```javascript
const getPixelRatio = (context) => {
  if (!context) {
    return 1;
  }
  const backingStore =
    context.backingStorePixelRatio ||
    context.webkitBackingStorePixelRatio ||
    context.mozBackingStorePixelRatio ||
    context.msBackingStorePixelRatio ||
    context.oBackingStorePixelRatio ||
    context.backingStorePixelRatio ||
    1;
  return (window.devicePixelRatio || 1) / backingStore;
};

class Watermark {
  constructor(player) {
    this.player = player;

    this.init();
  }

  _getOption(waterOption) {
    const defaultOption = {
      render: false,
      width: 120,
      height: 64,
      rotate: -22,
      image: '',
      zIndex: 9,

      content: 'Dplayer',
      fontColor: 'rgba(0,0,0,.15)',
      fontSize: 16,
      fontWeight: 'normal', // 'normal' | 'light' | 'weight' | number
      fontFamily: 'sans-serif',
      fontStyle: 'normal', // 'none' | 'normal' | 'italic' | 'oblique'

      gapX: 212,
      gapY: 222,
      offsetLeft: undefined,
      offsetTop: undefined,
    };

    for (const defaultKey in defaultOption) {
      if (defaultOption.hasOwnProperty(defaultKey) && !waterOption.hasOwnProperty(defaultKey)) {
        waterOption[defaultKey] = defaultOption[defaultKey];
      }
    }

    this.waterOption = waterOption;

    return waterOption;
  }

  init(option) {
    const waterOption = option || this.player.options.watermark || {};
    this.waterOption = this._getOption(waterOption);

    const {
      zIndex,
      gapX,
      gapY,
      offsetLeft,
      offsetTop,
      rotate,
      fontStyle,
      fontWeight,
      width,
      height,
      fontFamily,
      fontColor,
      image,
      content,
      fontSize,
      render,
    } = this.waterOption;

    if (!render) {
      return;
    }

    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');
    const ratio = getPixelRatio(ctx);

    const canvasWidth = `${(gapX + width) * ratio}px`;
    const canvasHeight = `${(gapY + height) * ratio}px`;
    const canvasOffsetLeft = offsetLeft || gapX / 2;
    const canvasOffsetTop = offsetTop || gapY / 2;

    canvas.setAttribute('width', canvasWidth);
    canvas.setAttribute('height', canvasHeight);

    if (ctx) {
      // 旋转字符 rotate
      ctx.translate(canvasOffsetLeft * ratio, canvasOffsetTop * ratio);
      ctx.rotate((Math.PI / 180) * Number(rotate));
      const markWidth = width * ratio;
      const markHeight = height * ratio;

      if (image) {
        const img = new Image();
        img.crossOrigin = 'anonymous';
        img.referrerPolicy = 'no-referrer';
        img.src = image;
        img.onload = () => {
          ctx.drawImage(img, 0, 0, markWidth, markHeight);
          this.setBase64Url(canvas.toDataURL(), { zIndex, gapX, width });
        };
      } else if (content) {
        const markSize = Number(fontSize) * ratio;
        ctx.font = `${fontStyle} normal ${fontWeight} ${markSize}px/${markHeight}px ${fontFamily}`;
        // console.log(`${fontStyle} normal ${fontWeight} ${markSize}px/${markHeight}px ${fontFamily}`)
        ctx.fillStyle = fontColor;
        if (Array.isArray(content)) {
          content.forEach((item, index) => ctx.fillText(item, 0, index * 50));
        } else {
          ctx.fillText(content, 0, 0);
        }
        this.setBase64Url(canvas.toDataURL(), { zIndex, gapX, width });
      }
    } else {
      console.error('当前环境不支持Canvas');
    }
  }

  setBase64Url(base64Url, { zIndex, gapX, width }) {
    const cls = document.querySelector('.dplayer-watermark-cls');

    if (!cls) {
      return;
    }

    cls.style.zIndex = zIndex;
    cls.style.backgroundImage = `url('${base64Url}')`;
    cls.style.backgroundSize = `${gapX + width}px`;
    cls.style.backgroundRepeat = 'repeat';
  }

  clear() {
    const cls = document.querySelector('.dplayer-watermark-cls');

    if (!cls) {
      return;
    }

    cls.parentElement.removeChild(cls);
  }
}

const watermark = {
    render: true,
    content: '某某某正在观看视频',
    fontSize: 14,
    gapX: 230,
    gapY: 200,
    rotate: -15,
    fontColor: "rgba(176,196,222, .15)",
},
```