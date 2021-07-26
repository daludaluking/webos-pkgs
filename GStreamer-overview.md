---
marp: true
paginate: false
backgroundColor: white
---

<style>
h1 {
  font-size:  1.9rem;
}
h3 {
  line-height: 1.2
}
section.lead h1 {
  text-align: center;
}
section.lead h2 {
  text-align: center;
}
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

<!--
theme: default
class: lead
-->

<!-- paginate: true -->
# GStreamer & QTIQMMFSrc Element Overview

---

## GStreamer란?

<br>

### - Multimedia application을 위한 framework
### - Linux, Windows, OS X, Android 등 지원...
### - 20년 이상된 opensource project

---

## Feature

<br>

### - Plugin으로 확장 가능, 유연함
### - Often wraps other libraries (decoders, encoders, filters, etc.)
### - Pipeline-based
### - Bindings to multiple languages (C/C++, Python, Java, Ruby, Pearl, etc.)

---

## GStreamer Overall Architecture

<br>

![center](gstreamer-overview.png)

---

## GStreamer Pipeline Example

<br>

![center](gstreamer-pipeline.png)

---

## Element

### - GStreamer에서 가장 중요한 class object
### - Element들은 자신만의 명시된 기능을 지원
### - Element들은 서로 연결되고 data가 전달됨
### - GStreamer는 많은 elements들을 제공
### - Element 종류 : Source, sink, filter, converters, demuxers, muxers, codecs

<br>

![center](gstreamer-elements.png)

---

## Pads

### - Element들 간의 연결지점(connection points)
![](gstreamer-pads.png)
### - Src(source) pads는 data를 생성.
### - Sink pads는 data를 소비 (수신함).
### - Data는 !항상! src pads에서 sink pads로 흐름.
### - pull or push mode로 동작.

---

## Pads (Cont)

### - Pad는 Capabilities or Caps라는 속성(property)을 가짐
### - Caps는 element들 간의 연결을 검증하는데 사용됨 (data의 type을 제한함)
### - Src pad와 sink pad는 각 pad들이 허용하는 data type이 호환되는 경우에 연결
- 예를 들어, video stream을 생성하는 src pad는 audio stream을 수신하는 sink pad와 연결될 수 없음
### - Elements는 사용할 형식에 대해 서로 협상할 수 있음 (caps negotiation)

---

## Pads (Cont)

<br>

![center](gstreamer-pads-2.png)

---

## Bin


### - Element들은 bin이라는 컨테이너로 그룹화 될 수 있음

<br>

![center](gstreamer-bin.png)

---

## Pipeline

### - A top-level bin
### - Communication을 위해 bus를 제공
### - 재생 동기화를 관리함.
### - 분리된 thread에서 실행됨.

<br>

![center](gstreamer-pipeline-2.png)

---

## Bus for communication

<br>

![center](gstreamer-bus.png)

---

## Pipeline Example 1

<br>

![center](gstreamer-pipeline-ex1.png)

---

## Pipeline Example 2

<br>

![center](gstreamer-pipeline-ex2.png)

---

# QTIQMMFSrc Element

---

## QTIQMMFSrc 란?

### - GST SRC plugin (qtiqmmfsrc) 

![center](gstreamer-qti-camera.png)

---

## Feature

### - qtiqmmfsrc는 AVC/HEVC bitstreames 및 YUV streams을 제공
### - qtiqmmfsrc는 QMMF server의 client, QMMF server는 deamon
### - QMMF server는 binder RPC 기반의 camera/recorder use cases를 구현한 deamon
### - QMMF-SDK interacts with HAL3 and further HAL3 interacts with camera backend (Camx)

---
## Video encoder usecase

![center](gstreamer-qti-camera-video.png)

---

## qtiqmmfsrc

![center](gstreamer-qtiqmmfsrc-el.png)

### - To capture video frame via the QMMF service
### - A wrapper on top of the QMMF Recorder Client
### - 2 pads for video and image streams.
### - Pads will push the buffer to its linked sink pad from the next plugin.

---

## Problem

### - qtiqmmfsrc는 QMMF *Recorder* client
### - QCS605 QMMF와 qtiqmmfsrc (based QCS610) 의 호환성 문제
### - qtiqmmfsrc는 Preview를 위해 GBM을 사용.
### - 현재 webos 버전에 QCT LE porting이 완전하지 않음.

---

## Modified qtiqmmfsrc (Cont)

![center](gstreamer-qtiqmmfsrc-with-src.png)

### - Add src pad for preview data
### - image_%u : for capture a single image buffer
### - video_%u : for streaming video data

--- 

## Modified qtiqmmfsrc

![width:340px center](gstreamer-qtiqmmfsrc-with-camera-client.png)

---

## Reference

### 1. [GStreamer Tutorials : https://gstreamer.freedesktop.org/documentation/tutorials/basic/index.html?gi-language=c](https://gstreamer.freedesktop.org/documentation/tutorials/basic/index.html?gi-language=c)

### 2. [GStreamer Writer's Guide : https://gstreamer.freedesktop.org/documentation/plugin-development/index.html?gi-language=c](https://gstreamer.freedesktop.org/documentation/plugin-development/index.html?gi-language=c)

### 3. QCS610/QCS410 Linux Platform Development Kit : 80-pl631-100_m_qcs610_qcs410_linux_platform_development_kit_software_reference_manual.pdf