<a id="Overview"></a>

# 1 概述

PLWinPlayer 是七牛推出的一款免费的适用于 Windows 平台的播放器 SDK，采用全自研的跨平台播放内核，拥有丰富的功能和优异的性能，可高度定制化和二次开发。

SDK 的 Github 地址：https://github.com/pili-engineering/PLWinPlayer

## 1.1 特性

- [x] 支持 RTMP 和 HLS 协议的直播流媒体播放
- [x] 支持常见的音视频文件播放（MP4、mp3、flv 等）
- [x] 支持播放器音量设置，可实现静音功能
- [x] 支持纯音频播放
- [x] 支持后台播放
- [x] 支持首屏秒开
- [x] 支持直播累积延时优化
- [x] 支持带 IP 地址的播放 URL
- [x] 支持 HTTPS 协议
- [x] 支持自动重连
- [x] 支持 H.265 播放
- [x] 支持七牛私有 DRM
- [x] 支持边下边播
- [x] 支持 mp4 本地缓存功能
- [x] 支持音视频数据回调
- [x] 支持自定义音视频渲染
- [x] 支持倍数播放功能（0.5x，1x，2x，4x 等）
- [x] 支持 HLS 自适应码率切换播放


<a id="Audience"></a>

# 2 阅读对象

 本文档为技术文档，需要阅读者：

- 具有基本的 Windows 开发能力
- 具有基本的音视频播放知识

<a id="APIGuide"></a>

# 3 API 介绍

## 3.1 函数说明

- **int QC_API qcCreatePlayer(QCM_Player * fPlayer)**

```
@function qcCreatePlayer
@abstract Create player session and get interface function address
@param  fPlayer [in][out] The structure of function address and session handle
@return QC_ERR_NONE if successful
```

- **int QC_API qcDestroyPlayer(QCM_Player * fPlayer)**

```
@function qcDestroyPlayer
@abstract Destroy player session
@param  fPlayer [in] The structure of function address and session handle
@return QC_ERR_NONE if successful
```
- **int SetNotify(void * hPlayer, QCPlayerNotifyEvent pFunc, void * pUserData)**

```
@function SetNotify
@abstract Register player event callback
@param  hPlayer [in] The player session handle
@param  pFunc [in] The callback function
@param  pUserData [in] The user data used in callback function
@return QC_ERR_NONE if successful
```
- **int SetView(void * hPlayer, void * hView, RECT * pRect)**

```
@function SetView
@abstract Set the view to render video
@param  hPlayer [in] The player session handle
@param  hView [in] The view handle
@param  pRect [in] The area of video
@return QC_ERR_NONE if successful
```
- **int Open(void * hPlayer, const char * pURL, int nFlag)**

```
@function Open
@abstract Open a clip
@param  hPlayer [in] The player session handle
@param  pURL [in] The URL of clip
@param  nFlag [in] The open flag, right now it supports QCPLAY_OPEN_VIDDEC_HW
@return QC_ERR_NONE if successful
```
- **int Close(void * hPlayer)**

```
@function Close
@abstract Close player
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
- **int Run(void * hPlayer)**

```
@function Run
@abstract Begin to play
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
- **int Pause(void * hPlayer)**

```
@function Pause
@abstract Pause player
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
- **int Stop(void * hPlayer)**

```
@function Stop
@abstract Stop player
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
- **QCPLAY_STATUS GetStatus(void * hPlayer)**

```
@function GetStatus
@abstract Get player's status
@param  hPlayer [in] The player session handle
@return QCPLAY_STATUS, it includes play, pause, stop, etc
```
- **long long GetDur(void * hPlayer)**

```
@function GetDur
@abstract Get clip's duration
@param  hPlayer [in] The player session handle
@return duration
```
- **long long GetPos(void * hPlayer)**

```
@function GetPos
@abstract Get current play time
@param  hPlayer [in] The player session handle
@return playing time
```
- **long long SetPos(void * hPlayer, long long llPos)**

```
@function SetPos
@abstract Seek to new position
@param  hPlayer [in] The player session handle
@param  llPos [in] The new position
@return the actual position
```
- **int SetVolume(void * hPlayer, int nVolume)**

```
@function SetVolume
@abstract Adjust audio volume
@param  hPlayer [in] The player session handle
@param  nVolume [in] The new volume value, 0~100
@return QC_ERR_NONE if successful
```
- **int GetVolume(void * hPlayer)**

```
@function GetVolume
@abstract Get current audio volume
@param  hPlayer [in] The player session handle
@return current volume value, 0~100
```
- **int GetParam(void * hPlayer, int nID, void * pParam)**

```
@function GetParam
@abstract Get parameter value from player
@param  nId [in] The parameter ID, refer to parameter ID definition
@param  pParam [in][out] The output value
@return QC_ERR_NONE if successful 
```
- **int SetParam(void * hPlayer, int nID, void * pParam)**

```
@function SetParam
@abstract Set parameter value
@param  nId [in] The parameter ID, refer to parameter ID definition
@param  pParam [in] The input value
@return QC_ERR_NONE if successful 
```
- **typedef int  (QC_API * QCPlayerOutAVData) (void * pUserData, QC_DATA_BUFF * pBuffer)**

```
@function QCPlayerOutAVData
@abstract Call back function of player send out video or audio render data
@param  pUserData [out] User data
@param  QC_DATA_BUFF [in/out] Audio/video render data. pBuffer field nValue is 11 means player don't need render
@return QC_ERR_NONE if successful 
```
##3.2 消息回调

- **QC_MSG_PLAY_OPEN_DONE 0x16000001**

```
@event QC_MSG_PLAY_OPEN_DONE
@abstract The opening of clip is completed, it can call Play function right now
```
- **QC_MSG_PLAY_OPEN_FAILED 0x16000002**

```
@event QC_MSG_PLAY_OPEN_FAILED
@abstract It failed to open clip
```
- **QC_MSG_PLAY_SEEK_DONE 0x16000005**

```c
@event QC_MSG_PLAY_SEEK_DONE
@abstract The seek operation is completed
```
- **QC_MSG_PLAY_SEEK_FAILED 0x16000006**

```
@event QC_MSG_PLAY_SEEK_FAILED
@abstract It failed to seek clip 
```
- **QC_MSG_PLAY_COMPLETE 0x16000007**

```
@event QC_MSG_PLAY_COMPLETE
@abstract Playback reaches the end of stream
```
- **QC_MSG_PLAY_CAPTURE_IMAGE 0x16000010**

```
@event QC_MSG_PLAY_CAPTURE_IMAGE
@abstract Output image data
@param QC_DATA_BUFF*, pBuffer is data pointer, uSize is data size
```
- **QC_MSG_BUFF_VBUFFTIME 0x18000001**

```
@event QC_MSG_BUFF_VBUFFTIME
@abstract Video buffer length in ms
@param int*
```
- **QC_MSG_BUFF_ABUFFTIME 0x18000002**

```
@event QC_MSG_BUFF_ABUFFTIME
@abstract Audio buffer length in ms
@param int*
```
- **QC_MSG_BUFF_START_BUFFERING  0x18000016**

```
@event QC_MSG_BUFF_START_BUFFERING
@abstract Start buffering
```
- **QC_MSG_BUFF_END_BUFFERING 0x18000017**

```
@event QC_MSG_BUFF_END_BUFFERING
@abstract Buffering end
```
- **QC_MSG_SNKV_NEW_FORMAT 0x15200003**

```
@event QC_MSG_SNKV_NEW_FORMAT
@abstract The video size was changed. The player need to resize the surface pos and size.
```
- **QC_MSG_SNKA_NEW_FORMAT 0x15100003**

```
@event QC_MSG_SNKA_NEW_FORMAT
@abstract The audio size was changed.
```
## 3.3 参数配置

- **QC_PLAY_BASE 0X11000000**

```
@id QC_PLAY_BASE
@abstract The base parameter ID value
```
- **QCPLAY_PID_Speed QC_PLAY_BASE + 0X02**

```
@id QCPLAY_PID_Speed
@abstract Set playback speed, double*, it is 0.2 - 32.0
```
- **QCPLAY_PID_Disable_Video QC_PLAY_BASE + 0X03**

```
@id QCPLAY_PID_Disable_Video
@abstract Disable video render, just audio playback
```
- **QCPLAY_PID_StreamNum QC_PLAY_BASE + 0X0b**

```
@id QCPLAY_PID_StreamNum
@abstract Get stream count, int*
```
- **QCPLAY_PID_StreamPlay QC_PLAY_BASE + 0X0c**

```
@id QCPLAY_PID_StreamPlay
@abstract Play the selected stream, index of stream, int*
```
- **QCPLAY_PID_StreamInfo QC_PLAY_BASE + 0X0f**

```
@id QCPLAY_PID_StreamInfo
@abstract Get stream's propertied, for example bitrate etc, QC_STREAM_FORMAT*
```
- **QCPLAY_PID_Socket_ConnectTimeout QC_PLAY_BASE + 0X0200**

```
@id QCPLAY_PID_Socket_ConnectTimeout
@abstract Set / Get the socket connect timeout time in ms, int*
```
- **QCPLAY_PID_Socket_ReadTimeout QC_PLAY_BASE + 0X0201**

```
@id QCPLAY_PID_Socket_ReadTimeout
@abstract Set / Get the socket read timeout time in ms, int*
```
- **QCPLAY_PID_HTTP_HeadReferer QC_PLAY_BASE + 0X0205**

```
@id QCPLAY_PID_HTTP_HeadReferer
@abstract Set the http header referer, char*
```
- **QCPLAY_PID_PlayBuff_MaxTime QC_PLAY_BASE + 0X0211**

```
@id QCPLAY_PID_PlayBuff_MaxTime
@abstract Set / get the max buffer time in ms, int*
```
- **QCPLAY_PID_PlayBuff_MinTime QC_PLAY_BASE + 0X0212**

```
@id QCPLAY_PID_PlayBuff_MinTime
@abstract Set / get the max buffer time in ms, int*
```
- **QCPLAY_PID_DRM_KeyText QC_PLAY_BASE + 0X0301**

```
@id QCPLAY_PID_DRM_KeyText
@abstract Set the DRM key, char*
```
- **QCPLAY_PID_Capture_Image QC_PLAY_BASE + 0X0310**

```
@id QCPLAY_PID_Capture_Image
@abstract Set to capture video image, long long*, capture time, 0 is immediatily
```
- **QCPLAY_PID_Log_Level QC_PLAY_BASE + 0X0320**

```
@id QCPLAY_PID_Log_Level
@abstract Set the log out level, int*. 0 none, 1 error, 2 warning, 3 info, 4 debug
```
- **QCPLAY_PID_SendOut_VideoBuff QC_PLAY_BASE + 0X0330**


```
@id QCPLAY_PID_SendOut_VideoBuff
@abstract Set to call back video buffer. It should be set after open before run. 
@ param is callback function QCPlayerOutAVData, video buffer is in QC_DATA_BUFF. pBuffPtr is pointer of struct QC_VIDEO_BUFF
```
- **QCPLAY_PID_SendOut_AudioBuff QC_PLAY_BASE + 0X0331**

```
@id QCPLAY_PID_SendOut_AudioBuff
@abstract Set to call back audio buffer. It should be set after open before run. 
@ param is callback function QCPlayerOutAVData, video buffer is in QC_DATA_BUFF. pBuff is audio data, uSize is audio size
```
# 4 历史记录

- 1.1.0.56 ([Release Notes][1])
  - 发布 PLWinPlayer v1.1.0.56

<a id="Issues"></a>

# 5 反馈及意见

当你遇到任何问题时，可以通过在 GitHub 的 repo 提交 issues 来反馈问题，请尽可能的描述清楚遇到的问题，如果有错误信息也一同附带，并且在 Labels 中指明类型为 bug 或者其他。

通过这里查看已有的 issues 和提交 Bug。

[1]: https://github.com/pili-engineering/PLWinPlayer/blob/master/ReleaseNote/release-notes-1.1.0.56.md