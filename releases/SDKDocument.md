_**This document describes the API of Qiniu core player SDK on Android and iOS, it includes three parts below**_
```
1. API
2. Event ID
3. Parameter ID
```
#一、API on Windows

```
@function qcCreatePlayer
@abstract Create player session and get interface function address
@param  fPlayer [in][out] The structure of function address and session handle
@return QC_ERR_NONE if successful
```
##int QC_API qcCreatePlayer(QCM_Player * fPlayer);

```
@function qcDestroyPlayer
@abstract Destroy player session
@param  fPlayer [in] The structure of function address and session handle
@return QC_ERR_NONE if successful
```
##int QC_API qcDestroyPlayer(QCM_Player * fPlayer);

```
@function SetNotify
@abstract Register player event callback
@param  hPlayer [in] The player session handle
@param  pFunc [in] The callback function
@param  pUserData [in] The user data used in callback function
@return QC_ERR_NONE if successful
```
##int SetNotify(void * hPlayer, QCPlayerNotifyEvent pFunc, void * pUserData);

```
@function SetView
@abstract Set the view to render video
@param  hPlayer [in] The player session handle
@param  hView [in] The view handle
@param  pRect [in] The area of video
@return QC_ERR_NONE if successful
```
##int SetView(void * hPlayer, void * hView, RECT * pRect);

```
@function Open
@abstract Open a clip
@param  hPlayer [in] The player session handle
@param  pURL [in] The URL of clip
@param  nFlag [in] The open flag, right now it supports QCPLAY_OPEN_VIDDEC_HW
@return QC_ERR_NONE if successful
```
##int Open(void * hPlayer, const char * pURL, int nFlag);

```
@function Close
@abstract Close player
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
##int Close(void * hPlayer);

```
@function Run
@abstract Begin to play
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
##int Run(void * hPlayer);

```
@function Pause
@abstract Pause player
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
##int Pause(void * hPlayer);

```
@function Stop
@abstract Stop player
@param  hPlayer [in] The player session handle
@return QC_ERR_NONE if successful
```
##int Stop(void * hPlayer);

```
@function GetStatus
@abstract Get player's status
@param  hPlayer [in] The player session handle
@return QCPLAY_STATUS, it includes play, pause, stop, etc
```
##QCPLAY_STATUS GetStatus(void * hPlayer);

```
@function GetDur
@abstract Get clip's duration
@param  hPlayer [in] The player session handle
@return duration
```
##long long GetDur(void * hPlayer);

```
@function GetPos
@abstract Get current play time
@param  hPlayer [in] The player session handle
@return playing time
```
##long long GetPos(void * hPlayer);

```
@function SetPos
@abstract Seek to new position
@param  hPlayer [in] The player session handle
@param  llPos [in] The new position
@return the actual position
```
##long long SetPos(void * hPlayer, long long llPos);

```
@function SetVolume
@abstract Adjust audio volume
@param  hPlayer [in] The player session handle
@param  nVolume [in] The new volume value, 0~100
@return QC_ERR_NONE if successful
```
##int SetVolume(void * hPlayer, int nVolume);

```
@function GetVolume
@abstract Get current audio volume
@param  hPlayer [in] The player session handle
@return current volume value, 0~100
```
##int GetVolume(void * hPlayer);

```
@function GetParam
@abstract Get parameter value from player
@param  nId [in] The parameter ID, refer to parameter ID definition
@param  pParam [in][out] The output value
@return QC_ERR_NONE if successful 
```
##int GetParam(void * hPlayer, int nID, void * pParam);

```
@function SetParam
@abstract Set parameter value
@param  nId [in] The parameter ID, refer to parameter ID definition
@param  pParam [in] The input value
@return QC_ERR_NONE if successful 
```
##int SetParam(void * hPlayer, int nID, void * pParam);

```
@function QCPlayerOutAVData
@abstract Call back function of player send out video or audio render data
@param  pUserData [out] User data
@param  QC_DATA_BUFF [in/out] Audio/video render data. pBuffer field nValue is 11 means player don't need render
@return QC_ERR_NONE if successful 
```
##typedef int  (QC_API * QCPlayerOutAVData) (void * pUserData, QC_DATA_BUFF * pBuffer);


#二、Event on Windows

```
@event QC_MSG_PLAY_OPEN_DONE
@abstract The opening of clip is completed, it can call Play function right now
```
## #define QC_MSG_PLAY_OPEN_DONE 0x16000001

```
@event QC_MSG_PLAY_OPEN_FAILED
@abstract It failed to open clip
```
## #define QC_MSG_PLAY_OPEN_FAILED 0x16000002

```
@event QC_MSG_PLAY_SEEK_DONE
@abstract The seek operation is completed
```
## #define QC_MSG_PLAY_SEEK_DONE 0x16000005

```
@event QC_MSG_PLAY_SEEK_FAILED
@abstract It failed to seek clip
```
## #define QC_MSG_PLAY_SEEK_FAILED 0x16000006

```
@event QC_MSG_PLAY_COMPLETE
@abstract Playback reaches the end of stream
```
## #define QC_MSG_PLAY_COMPLETE			0x16000007

```
@event QC_MSG_PLAY_CAPTURE_IMAGE
@abstract Output image data
@param QC_DATA_BUFF*, pBuffer is data pointer, uSize is data size
```
## #define QC_MSG_PLAY_CAPTURE_IMAGE			0x16000010

```
@event QC_MSG_BUFF_VBUFFTIME
@abstract Video buffer length in ms
@param int*
```
## #define QC_MSG_BUFF_VBUFFTIME			0x18000001

```
@event QC_MSG_BUFF_ABUFFTIME
@abstract Audio buffer length in ms
@param int*
```
## #define QC_MSG_BUFF_ABUFFTIME			0x18000002

```
@event QC_MSG_BUFF_START_BUFFERING
@abstract Start buffering
```
## #define QC_MSG_BUFF_START_BUFFERING			0x18000016

```
@event QC_MSG_BUFF_END_BUFFERING
@abstract Buffering end
```
## #define QC_MSG_BUFF_END_BUFFERING			0x18000017

```
@event QC_MSG_SNKV_NEW_FORMAT
@abstract The video size was changed. The player need to resize the surface pos and size.
```
## #define QC_MSG_SNKV_NEW_FORMAT 0x15200003

```
@event QC_MSG_SNKA_NEW_FORMAT
@abstract The audio size was changed.
```
## #define QC_MSG_SNKA_NEW_FORMAT 0x15100003

#三、Parameter ID on Windows

```
@id QC_PLAY_BASE
@abstract The base parameter ID value
```
## #define QC_PLAY_BASE 0X11000000

```
@id QCPLAY_PID_Speed
@abstract Set playback speed, double*, it is 0.2 - 32.0
```
## #define	QCPLAY_PID_Speed QC_PLAY_BASE + 0X02

```
@id QCPLAY_PID_Disable_Video
@abstract Disable video render, just audio playback
```
## #define	QCPLAY_PID_Disable_Video QC_PLAY_BASE + 0X03

```
@id QCPLAY_PID_StreamNum
@abstract Get stream count, int*
```
## #define	QCPLAY_PID_StreamNum	 QC_PLAY_BASE + 0X0b

```
@id QCPLAY_PID_StreamPlay
@abstract Play the selected stream, index of stream, int*
```
## #define	QCPLAY_PID_StreamPlay QC_PLAY_BASE + 0X0c

```
@id QCPLAY_PID_StreamInfo
@abstract Get stream's propertied, for example bitrate etc, QC_STREAM_FORMAT*
```
## #define	QCPLAY_PID_StreamInfo QC_PLAY_BASE + 0X0f

```
@id QCPLAY_PID_Socket_ConnectTimeout
@abstract Set / Get the socket connect timeout time in ms, int*
```
## #define	QCPLAY_PID_Socket_ConnectTimeout QC_PLAY_BASE + 0X0200

```
@id QCPLAY_PID_Socket_ReadTimeout
@abstract Set / Get the socket read timeout time in ms, int*
```
## #define	QCPLAY_PID_Socket_ReadTimeout QC_PLAY_BASE + 0X0201

```
@id QCPLAY_PID_HTTP_HeadReferer
@abstract Set the http header referer, char*
```
## #define	QCPLAY_PID_HTTP_HeadReferer QC_PLAY_BASE + 0X0205

```
@id QCPLAY_PID_PlayBuff_MaxTime
@abstract Set / get the max buffer time in ms, int*
```
## #define	QCPLAY_PID_PlayBuff_MaxTime QC_PLAY_BASE + 0X0211

```
@id QCPLAY_PID_PlayBuff_MinTime
@abstract Set / get the max buffer time in ms, int*
```
## #define	QCPLAY_PID_PlayBuff_MinTime QC_PLAY_BASE + 0X0212

```
@id QCPLAY_PID_DRM_KeyText
@abstract Set the DRM key, char*
```
## #define	QCPLAY_PID_DRM_KeyText QC_PLAY_BASE + 0X0301

```
@id QCPLAY_PID_Capture_Image
@abstract Set to capture video image, long long*, capture time, 0 is immediatily
```
## #define	QCPLAY_PID_Capture_Image QC_PLAY_BASE + 0X0310

```
@id QCPLAY_PID_Log_Level
@abstract Set the log out level, int*. 0 none, 1 error, 2 warning, 3 info, 4 debug
```
## #define	QCPLAY_PID_Log_Level QC_PLAY_BASE + 0X0320


```
@id QCPLAY_PID_SendOut_VideoBuff
@abstract Set to call back video buffer. It should be set after open before run. 
@ param is callback function QCPlayerOutAVData, video buffer is in QC_DATA_BUFF. pBuffPtr is pointer of struct QC_VIDEO_BUFF
```
## #define	QCPLAY_PID_SendOut_VideoBuff QC_PLAY_BASE + 0X0330

```
@id QCPLAY_PID_SendOut_AudioBuff
@abstract Set to call back audio buffer. It should be set after open before run. 
@ param is callback function QCPlayerOutAVData, video buffer is in QC_DATA_BUFF. pBuff is audio data, uSize is audio size
```
## #define	QCPLAY_PID_SendOut_AudioBuff QC_PLAY_BASE + 0X0331


