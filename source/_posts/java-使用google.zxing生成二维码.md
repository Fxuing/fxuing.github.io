---
title: java-使用google.zxing生成二维码
date: 2020年6月16日
comments: true
categories: java
tags:
- 二维码
---

### 1. 导入maven依赖

```java
<dependency>
    <groupId>com.google.zxing</groupId>
    <artifactId>core</artifactId>
    <version>3.1.0</version>
</dependency>
<dependency>
    <groupId>com.google.zxing</groupId>
    <artifactId>javase</artifactId>
    <version>3.1.0</version>
</dependency>
```

<!--more-->

### 2. 二维码工具类

```java
package com.odp.api.common.utils;

import com.google.zxing.BarcodeFormat;
import com.google.zxing.EncodeHintType;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.common.BitMatrix;
import com.google.zxing.qrcode.decoder.ErrorCorrectionLevel;

import java.awt.image.BufferedImage;
import java.util.Hashtable;

/**
 * @Author: hou_fx
 * @Description:
 * @Data: created in  下午6:02 2019/11/28
 */
public class QRCodeUtil {
    private static final String CHARSET = "utf-8";
    public static BufferedImage createImage(String content, int width, int height ) throws Exception {
        Hashtable<EncodeHintType, Object> hints = new Hashtable<EncodeHintType, Object>();
        hints.put(EncodeHintType.ERROR_CORRECTION, ErrorCorrectionLevel.H);
        hints.put(EncodeHintType.CHARACTER_SET, CHARSET);
        hints.put(EncodeHintType.MARGIN, 1);
        BitMatrix bitMatrix = new MultiFormatWriter().encode(content, BarcodeFormat.QR_CODE, width, height,
                hints);
        width = bitMatrix.getWidth();
        height = bitMatrix.getHeight();
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        for (int x = 0; x < width; x++) {
            for (int y = 0; y < height; y++) {
                image.setRGB(x, y, bitMatrix.get(x, y) ? 0xFF000000 : 0xFFFFFFFF);
            }
        }
        return image;
    }
}
```

### 3. 生成二维码图片

    调用工具类生成二维码，返回BufferedImage,根据需求将图片转为需要的数据,如转为base64等、

```java
ByteArrayOutputStream baos = new ByteArrayOutputStream();
BufferedImage image = QRCodeUtil.createImage(targetUrl, 460, 460);
ImageIO.write(image, "PNG", baos);
Base64.getEncoder().encodeToString(baos.toByteArray());
```

### 4. 效果图

    在浏览器中，可用 data:image/png;base64 ,查看base64图片,如

![](C:\Users\HOUFUXING\AppData\Roaming\marktext\images\2020-06-17-18-39-53-image.png)