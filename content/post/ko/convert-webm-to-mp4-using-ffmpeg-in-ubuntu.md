---
title: Ubuntu 22.04에서 ScreenCast WebM 영상을 MP4로 변환하기
description: FFmpeg를 이용해 WebM을 MP4로 변환하는 방법을 설명합니다.
date: 2025-02-14T09:46:00
created: 2025-02-14T09:46:00
updated: 2025-02-19
tags:
  - ffmpeg
  - video-conversion
  - ubuntu
  - webm
  - mp4
categories: Media
draft: false
---

# Ubuntu 22.04에서 ScreenCast WebM 영상을 MP4로 변환하기

Ubuntu 22.04 환경에서 Screen Cast 기능을 이용해 녹화한 영상은 기본적으로 `.webm` 확장자로 저장된다. 하지만, 외부 공유나 편집을 위해 `.mp4` 확장자로 변환해야 할 때가 많다. 특히, Google Drive와 같은 플랫폼에서는 MP4가 더 널리 지원되므로 변환이 필요할 수 있다.

이 글에서는 FFmpeg를 이용하여 WebM 영상을 MP4로 변환하는 방법을 소개한다.

## FFmpeg란?

FFmpeg는 오디오 및 비디오를 처리하는 강력한 오픈소스 툴이다. 다양한 포맷 변환을 지원하며, 품질을 유지하면서 빠르게 영상을 변환할 수 있다.

## FFmpeg를 이용한 WebM → MP4 변환 방법

터미널에서 아래 명령어를 실행하면 `.webm` 파일을 `.mp4`로 변환할 수 있다.

```bash
ffmpeg -i <original_video.webm> -r 60 -vf "format=yuv444p,scale=1585:885" -c:v libx264 -crf 23 -c:a aac -strict -2 -q:a 100 -movflags +faststart <converted_video.mp4>
```

### 옵션 설명

- `-i <original_video.webm>`: 변환할 원본 WebM 파일을 지정한다.
- `-r 60`: 프레임 속도를 60fps로 설정하여 영상이 끊기지 않도록 한다.
- `-vf "format=yuv444p,scale=1585:885"`: 비디오 포맷을 YUV444p로 설정하고, 특정 해상도로 조정한다. (scale 값은 원본 영상에 맞춰 조정 필요)
- `-c:v libx264`: 비디오 코덱을 H.264로 설정한다.
- `-crf 23`: 비디오 품질을 설정한다. (값이 낮을수록 고품질, 18~28 범위 추천)
- `-c:a aac`: 오디오 코덱을 AAC로 설정한다.
- `-strict -2`: FFmpeg의 실험적 기능을 허용하여 AAC 코덱을 활성화한다.
- `-q:a 100`: 오디오 품질을 설정한다. (70~100 범위 추천)
- `-movflags +faststart`: MP4 파일의 메타데이터를 파일의 시작 부분으로 이동시켜 빠른 스트리밍이 가능하도록 한다.

## 변환 과정에서 발생할 수 있는 문제 해결

### 1. "width not divisible by 2" 오류

변환 중에 다음과 같은 오류가 발생할 수 있다.

```
width not divisible by 2
```

이는 영상의 가로 또는 세로 해상도가 홀수일 때 발생한다. 해결 방법은 `-vf "format=yuv444p,scale=1585:885"` 옵션을 추가하여 짝수 해상도로 변환하는 것이다.

출처: [Stack Overflow](https://stackoverflow.com/questions/60788967/ffmpeg-width-not-divisible-by-2-375x500-error)

### 2. 변환된 영상이 끊기는 문제

`-r 60` 옵션을 사용하지 않으면 영상이 뚝뚝 끊길 수 있다. 따라서 원활한 재생을 위해 이 옵션을 포함하는 것이 좋다.

## 변환 예시

다음은 실제 변환 과정에서 사용한 명령어이다.

```bash
ffmpeg -i Digital_Twin_BBS_View_UI_250108.webm -r 60 -vf "format=yuv444p,scale=1585:885" -c:v libx264 -crf 23 -c:a aac -strict -2 -q:a 100 -movflags +faststart test_output_250108.mp4
```

변환 후, MP4 파일을 Google Drive에 업로드하거나 편집 프로그램에서 활용할 수 있다.

## 마무리

Ubuntu 22.04에서 Screen Cast로 녹화한 WebM 영상을 MP4로 변환하는 방법을 알아보았다. FFmpeg는 강력한 툴이며, 다양한 옵션을 활용하면 원하는 설정으로 영상을 변환할 수 있다. 위의 명령어를 참고하여 필요에 맞게 변환을 진행해보자!

## 참고 자료

- [Stack Overflow - width not divisible by 2 오류 해결](https://stackoverflow.com/questions/60788967/ffmpeg-width-not-divisible-by-2-375x500-error)
- [FFmpeg 옵션 설명](https://ccusean.tistory.com/entry/webm%ED%8C%8C%EC%9D%BC%EC%9D%84-mp4%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0)

