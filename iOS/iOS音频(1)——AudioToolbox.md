>一、前言
二、音频文件Audio File Services
三、音频文件转换Extended Audio File Services
四、音频流Audio File Stream Services
五、音频队列Audio Queue Services

##一、前言
AudioToolbox提供的API主要是C 使用起来相对晦涩，针对本文提供了简单的代码示例减小学习的阻力 **[AudioToolbox](https://github.com/baxiang/AudioToolbox)**

![AudioToolbox](http://upload-images.jianshu.io/upload_images/143845-15b9d55ce60632ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![采样和采样率](http://upload-images.jianshu.io/upload_images/143845-d77216d1f6da181c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

sample 是一个声道的一个采样。采样率定义了每秒从连续信号中提取并组成离散信号的采样个数，它用赫兹（Hz）来表示。
![image.png](http://upload-images.jianshu.io/upload_images/143845-9b2f97b7892fcda6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
frame 是最小单位时间点包含的一个或多个声音采样，最小单位时间点取决于声音采样设备，是一个时间点多个采样的集合。譬如，双声道的音频文件，一个时间点有两个声道，一个Frames就包括两个采样。通道是声音的通道的数目。常有单声道和立体声之分。
![image.png](http://upload-images.jianshu.io/upload_images/143845-aff48030fcf79e47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
采样位数即采样值或取样值（就是将采样样本幅度量化）。它是用来衡量声音波动变化的一个参数，也可以说是声卡的分辨率。它的数值越大，分辨率也就越高，所发出声音的能力越强。每个采样数据记录的是振幅, 采样精度取决于采样位数的大小:
packet 是一个或多个 frame 的集合，一个 packet 包含多少个 frame，是由声音文件格式决定的。譬如 PCM 文件格式中一个 packet 包含 1 个frame。而 MP3 文件格式中一个 packet 包含 1152 个frames。
比特率：也称作位速/码率，是指在一个数据流中每秒钟能通过的信息量
比特率=采样频率×采样位数×声道数

##二、Audio File Services

####2.1、打开或关闭音频文件
```
   OSStatus AudioFileOpenURL ( CFURLRef inFileRef, AudioFilePermissions inPermissions, AudioFileTypeID inFileTypeHint, AudioFileID _Nullable *outAudioFile );

   NSString *path = [NSString stringWithFormat:@"%@",[[NSBundle mainBundle] pathForResource:@"MySong" ofType:@"mp3"]];
    AudioFileID audioFileID;
    OSStatus status = AudioFileOpenURL((__bridge CFURLRef _Nonnull)([NSURL fileURLWithPath:path]), kAudioFileReadPermission, 0, &audioFileID);
    if (status != noErr) {
        NSLog(@"文件读取失败 %d",status);
    }
```
 * CFURLRef 文件路径；
 *  AudioFilePermissions 文件读写权限 一般设置可读模式；
 *  inFileTypeHint  文件类型提示 未知设置0；
 * AudioFileID   文件句柄  
 AudioToolbox 函数的返回一般都是OSStatus  成功返回“noErr”，OSStatus常见错误
```
CF_ENUM(OSStatus) {
        kAudioFileUnspecifiedError						 = 'wht?',		// 0x7768743F, 2003334207
        kAudioFileUnsupportedFileTypeError 				= 'typ?',		// 0x7479703F, 1954115647
        kAudioFileUnsupportedDataFormatError 			  = 'fmt?',		// 0x666D743F, 1718449215
        kAudioFileUnsupportedPropertyError 				= 'pty?',		// 0x7074793F, 1886681407
        kAudioFileBadPropertySizeError 					= '!siz',		// 0x2173697A,  561211770
        kAudioFilePermissionsError	 					= 'prm?',		// 0x70726D3F, 1886547263
        kAudioFileNotOptimizedError						= 'optm',		// 0x6F70746D, 1869640813
        // file format specific error codes
        kAudioFileInvalidChunkError						= 'chk?',		// 0x63686B3F, 1667787583
        kAudioFileDoesNotAllow64BitDataSizeError		       = 'off?',		// 0x6F66663F, 1868981823
        kAudioFileInvalidPacketOffsetError				= 'pck?',		// 0x70636B3F, 1885563711
        kAudioFileInvalidFileError						= 'dta?',		// 0x6474613F, 1685348671
		kAudioFileOperationNotSupportedError			= 0x6F703F3F, 	// 'op??', integer used because of trigraph
		// general file error codes
		kAudioFileNotOpenError							= -38,
		kAudioFileEndOfFileError						= -39,
		kAudioFilePositionError							= -40,
		kAudioFileFileNotFoundError						= -43
};
```
查询 OSStatus错误解释的网站[OSStatus](https://www.osstatus.com/)
![image.png](http://upload-images.jianshu.io/upload_images/143845-9d97f3480009b35c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
与打开文件对应的close:
```
 @param inAudioFile 文件句柄
 OSStatus AudioFileClose ( AudioFileID inAudioFile );
```

####2.2、 读取音频属性

获得属性的总体的大小和属性是否可以修改
```
OSStatus AudioFileGetPropertyInfo ( AudioFileID inAudioFile, AudioFilePropertyID inPropertyID, UInt32 *outDataSize, UInt32 *isWritable );
```
在获得属性的具体内容
```
OSStatus AudioFileGetProperty ( AudioFileID inAudioFile, AudioFilePropertyID inPropertyID, UInt32 *ioDataSize, void *outPropertyData );
```
使用in开头函数代表只用作输入(inAudioFile 和inPropertyID，指定了获取哪个文件和哪个属性)，out开头的参数代表只用作输出（outPropertyData 指针指向的具体属性内容），io开头的参数既用作输入也用作输出（ioDataSize，接收你分配给outPropertyData的内存缓冲区的大小，然后返回实际上被写入缓冲区的大小），这种参数命名模式是AudioToolbox一个特点。可以提高对当前参数的理解。
对于isWritable为true的对其进行设置属性
```
OSStatus AudioFileSetProperty ( AudioFileID inAudioFile, AudioFilePropertyID inPropertyID, UInt32 inDataSize, const void *inPropertyData );
```


查询接口中也是一样，查询文件“inAudioFile”的“inPropertyID”的属性值，结果存放在长度为“ioDataSize”的buffer“outPropertyData”中。属性值有：

|AudioFilePropertyID	|意义|	结果类型|
| ------ |:-------:| :-----:|
|kAudioFilePropertyFileFormat|	音频文件的格式|	char *|
|kAudioFilePropertyDataFormat|	音频数据格式	|AudioStreamPacketDescription
|kAudioFilePropertyIsOptimized|	是否可以优化|	0/1
|kAudioFilePropertyMagicCookieData|	Magic Cookie文件头	|char *
|kAudioFilePropertyAudioDataByteCount	|文件长度	|Uint64
|kAudioFilePropertyAudioDataPacketCount	|Packet的数目	|Uint64
|kAudioFilePropertyMaximumPacketSize	|最大的Packet大小	|Uint32
|kAudioFilePropertyDataOffset	|数据的偏移量	|Uint64
|kAudioFilePropertyChannelLayout	|声道结构	|AudioFormatListItem
|kAudioFilePropertyDeferSizeUpdates	|是否更新文件头信息	|1/0
|kAudioFilePropertyMarkerList	|音频中所有markers	|CFStringRef表示的Markers列表
|kAudioFilePropertyRegionList	|音频中所有Region	|CFStringRef表示的Region列表
|kAudioFilePropertyPacketToFrame	|将包数转换成帧数	|AudioFramePacketTranslation中mPacket做输入，mFrame做输出
|kAudioFilePropertyFrameToPacket	|将帧数转换成包数	|AudioFramePacketTranslation中mFrame做输入，mFrameOffsetInPacket，mPacket做输出
|kAudioFilePropertyPacketToByte	|将包数转换成字节数	|AudioFramePacketTranslation中mPacket做输入，mByte做输出
|kAudioFilePropertyByteToPacket	|将字节数转换成包数	|AudioFramePacketTranslation中mByte做输入，mPacket和mByteOffsetInPacket做输出
|kAudioFilePropertyChunkIDs	|文件中的chunk编码格式|	4字符编码格式数组
|kAudioFilePropertyInfoDictionary	|字典表示的Info	|CFDictionary
|kAudioFilePropertyPacketTableInfo	|设置PacketTableInfo	|PacketTableInfo
|kAudioFilePropertyFormatList	|支持的格式列表	|编码格式list
|kAudioFilePropertyPacketSizeUpperBound	|理论上的最大Packet大小	|Uint64
|kAudioFilePropertyReserveDuration	|设置写保护区大小，单位为秒	|Uint32
|kAudioFilePropertyEstimatedDuration	|估算的音频时长 ， 单位秒	|Uint32
|kAudioFilePropertyBitRate	|码率	|Uint32
|kAudioFilePropertyID3Tag	|ID3 tag	|void *
|kAudioFilePropertySourceBitDepth	|位深度	|Uint32
|kAudioFilePropertyAlbumArtwork	|专辑名	|CFDataRef|
一些音频压缩的音频格式，例如 MPEG 4 AAC，利用结构体包含音频的元数据。这些结构体就是Magic Cookie，当你用 Audio Queue Services 播放这种格式的音频文件时，你可以从音频文件中获取Magic Cookie ，然后在播放之前添加到音频队列中
```
 UInt32 cookieSize = sizeof (UInt32);
    status =  AudioFileGetPropertyInfo (audioFileID,kAudioFilePropertyMagicCookieData,&cookieSize,NULL);
    if (!status && cookieSize) {
        char* magicCookie =(char *) malloc (cookieSize);
        AudioFileGetProperty (audioFileID,kAudioFilePropertyMagicCookieData,&cookieSize,magicCookie);
        AudioQueueSetProperty (inAQ,kAudioQueueProperty_MagicCookie,magicCookie,cookieSize);
        free (magicCookie);
    }
```
####2.3、 读取音频数据
AudioFileReadPackets 已经被废弃使用 不建议使用 主要使用的是AudioFileReadPacketData
```
OSStatus AudioFileReadBytes ( AudioFileID inAudioFile, Boolean inUseCache, SInt64 inStartingByte, UInt32 *ioNumBytes, void *outBuffer );
OSStatus AudioFileReadPacketData ( AudioFileID inAudioFile, Boolean inUseCache, UInt32 *ioNumBytes, AudioStreamPacketDescription *outPacketDescriptions, SInt64 inStartingPacket, UInt32 *ioNumPackets, void *outBuffer );
OSStatus AudioFileReadPackets ( AudioFileID inAudioFile, Boolean inUseCache, UInt32 *outNumBytes, AudioStreamPacketDescription *outPacketDescriptions, SInt64 inStartingPacket, UInt32 *ioNumPackets, void *outBuffer );// 已经废弃
```
*  AudioFileID inAudioFile 文件句柄
*  Boolean inUseCache 是否缓存读取的数据
*  UInt32 *outNumBytes ： 最终读到数据的大小
*  AudioStreamPacketDescription *outPacketDescriptions ： 一个存放AudioStreamPacketDescription的Buffer
*  SInt64 inStartingPacket ： 起始的Packet
*   UInt32 *ioNumPackets ： 当输入时表示要读取的Packet数目，输出时表示最终读入的Packet数目
*  void *outBuffer ： 数据读到的具体buffer位置

##三、Extended Audio File Services
Audio File Services提供的api 需要传入冗长的参数 Extended Audio File Services可以看做是对Audio File Services的封装，当时更多的实际开发我们用它来做音频文件类型的转换。
####3.1、打开和关闭音频数据
打开文件：
```
OSStatus ExtAudioFileOpenURL ( CFURLRef inURL, ExtAudioFileRef _Nullable *outExtAudioFile );
```
当操作完以后，通过Dispose来回收资源，区分于其他的Close:
```
OSStatus ExtAudioFileDispose ( ExtAudioFileRef inExtAudioFile );
```
####3.2、读取音频数据
和“Audio ToolBox”的其他属性操作一样，Ext接口提供的属性操作也是分为两步，先获取属性基本信息，如大小：
```
OSStatus ExtAudioFileGetPropertyInfo ( ExtAudioFileRef inExtAudioFile, ExtAudioFilePropertyID inPropertyID, UInt32 *outSize, Boolean *outWritable );
```
然后在获得属性内容：
```
OSStatus ExtAudioFileGetProperty ( ExtAudioFileRef inExtAudioFile, ExtAudioFilePropertyID inPropertyID, UInt32 *ioPropertyDataSize, void *outPropertyData );
```
或者设置属性内容：
```
OSStatus ExtAudioFileSetProperty ( ExtAudioFileRef inExtAudioFile, ExtAudioFilePropertyID inPropertyID, UInt32 inPropertyDataSize, const void *inPropertyData );
```
```
 _outputFormat.mSampleRate = 44100;
    _outputFormat.mBitsPerChannel = 16;
    _outputFormat.mChannelsPerFrame = 2;
    _outputFormat.mFormatID = kAudioFormatMPEGLayer3;
    
    UInt32 descSize = sizeof(AudioStreamBasicDescription);
    ExtAudioFileGetProperty(_audioFileRef, kExtAudioFileProperty_FileDataFormat, &descSize, &_inputFormat);
    
    
    _inputFormat.mSampleRate = _outputFormat.mSampleRate;
    _inputFormat.mChannelsPerFrame = _outputFormat.mChannelsPerFrame;
    _inputFormat.mBytesPerFrame = _inputFormat.mChannelsPerFrame* _inputFormat.mBytesPerFrame;
    _inputFormat.mBytesPerPacket =  _inputFormat.mFramesPerPacket*_inputFormat.mBytesPerFrame;
    

    ExtAudioFileSetProperty(_audioFileRef,
                            kExtAudioFileProperty_ClientDataFormat,
                            sizeof(AudioStreamBasicDescription),
                            &_inputFormat),

```
kExtAudioFileProperty_Xxxx : 源文件的相关属性，也就是原来什么格式的数据（MP3/AAC），他的基本属性。
kExtAudioFileProperty_ClientXxx: 读出时的数据格式，Ext在读出时会自动帮我们做编解码操作，这个是处理后的结果
所以在读取之前，一定要记得设置“kExtAudioFileProperty_ClientDataFormat”属性，设置其输出的数据格式，

|ExtAudioFilePropertyID|	意义|结果数据类型|	是否可读写|
|-----|------|-------|-----|
|kExtAudioFileProperty_FileDataFormat	|源音频数据的格式	|AudioStreamBasicDescription|	只读
|kExtAudioFileProperty_FileChannelLayout|	源音频数据的通道格式|	AudioChannelLayout|	读写
|kExtAudioFileProperty_ClientDataFormat|	读出来后的音频数据的格式|	AudioStreamBasicDescription|	读写
|kExtAudioFileProperty_ClientChannelLayout|	读出来后的音频数据的通道格式|	AudioChannelLayout|	读写
|kExtAudioFileProperty_CodecManufacturer|	是否使用硬件编解码|	UInt32（kAppleHardwareAudioCodecManufacturer or kAppleSoftwareAudioCodecManufacturer）|	读写
|kExtAudioFileProperty_AudioConverter	|指定的编解码工具|	AudioConverterRef	只读
|kExtAudioFileProperty_AudioFile	|对应的AudioFileID|	AudioFileID	|只读
|kExtAudioFileProperty_FileMaxPacketSize|	源音频数据最大的Packet大小|	Uint32|	只读
|kExtAudioFileProperty_ClientMaxPacketSize|	读出后音频数据最大的Packet大小|	Uint32|	只读
|kExtAudioFileProperty_FileLengthFrames|	帧数	SInt64|	只读
|kExtAudioFileProperty_ConverterConfig|	指定编解码器	|CFArray|	读写
|kExtAudioFileProperty_IOBufferSizeBytes|	编解码使用的缓冲区大小|	UInt32|	读写
|kExtAudioFileProperty_IOBuffer|	编解码使用的缓冲区|	void *	|读写
|kExtAudioFileProperty_PacketTable|	设置PacketTable	|AudioFilePacketTableInfo|	读写

```
struct AudioBufferList
{
    UInt32      mNumberBuffers;
    AudioBuffer mBuffers[1]; // this is a variable length array of mNumberBuffers elements

#if defined(__cplusplus) && CA_STRICT
public:
    AudioBufferList() {}
private:
    //  Copying and assigning a variable length struct is problematic so turn their use into a
    //  compile time error for eacy spotting.
    AudioBufferList(const AudioBufferList&);
    AudioBufferList&    operator=(const AudioBufferList&);
#endif

};
typedef struct AudioBufferList  AudioBufferList;

struct AudioBuffer
{
    UInt32              mNumberChannels;
    UInt32              mDataByteSize;
    void* __nullable    mData;
};
typedef struct AudioBuffer  AudioBuffer;
```
写入文件内容

写入和读取类似，只是要预先填好BufferList的内容：
```
OSStatus ExtAudioFileWrite ( ExtAudioFileRef inExtAudioFile, UInt32 inNumberFrames, const AudioBufferList *ioData );
```
同时写入还有个非阻塞的版本,当调用“ ExtAudioFileDispose ”会最终保证所有数据都写入到磁盘中。
```
OSStatus ExtAudioFileWriteAsync ( ExtAudioFileRef inExtAudioFile, UInt32 inNumberFrames, const AudioBufferList *ioData );
```
##四、Audio File Stream Services
对于网络音频文件 大多采用的是边读取边播放，这个时候就用到了Audio File Stream
####4.1、初始化音频流
```
extern OSStatus	
AudioFileStreamOpen (
							void * __nullable						inClientData,
							AudioFileStream_PropertyListenerProc	inPropertyListenerProc,
							AudioFileStream_PacketsProc				inPacketsProc,
                			AudioFileTypeID							inFileTypeHint,
                			AudioFileStreamID __nullable * __nonnull outAudioFileStream)
```
* inClientData上下文对象；
* AudioFileStream_PropertyListenerProc 在调用AudioFileStreamParseBytes歌曲信息的回调；
* AudioFileStream_PacketsProc 在调用AudioFileStreamParseBytes对音频数据的回调，主要用于音频帧的数据分类存储。
* AudioFileTypeID 文件类型的提示，如果无法确定类型可以传入0
* AudioFileStreamID，获取当前实例对应的AudioFileStreamID，使用其他AudioFileStream API需要传入。 

####4.2、读取音频流
```
extern OSStatus
AudioFileStreamParseBytes(	
								AudioFileStreamID				inAudioFileStream,
								UInt32							inDataByteSize,
								const void *					inData,
								AudioFileStreamParseFlags		inFlags)
```
* AudioFileStreamID，AudioFileStreamOpen获取的的AudioFileStreamID；
* inDataByteSize，解析的数据字节长度；
* inData，解析的数据；
* AudioFileStreamParseFlags说本次的解析和上一次解析是否是连续的关系，如果是连续的传入0，否则传kAudioFileStreamParseFlag_Discontinuity。

####4.3、解析文件格式信息
```
typedef void (*AudioFileStream_PropertyListenerProc)(
											void *							inClientData,
											AudioFileStreamID				inAudioFileStream,
											AudioFileStreamPropertyID		inPropertyID,
											AudioFileStreamPropertyFlags *	ioFlags);
```
根据当前的PropertyID调用AudioFileStreamGetProperty获取当前音频文件的具体信息
```
if (inPropertyID ==  kAudioFileStreamProperty_DataFormat) {
        UInt32 outDataSize = sizeof(AudioStreamBasicDescription);
        AudioFileStreamGetProperty(inAudioFileStream, inPropertyID,  &outDataSize, &_audioStreamDescription);
    }
```
```
typedef void (*AudioFileStream_PacketsProc)(
											void *							inClientData,
											UInt32							inNumberBytes,
											UInt32							inNumberPackets,
											const void *					inInputData,
											AudioStreamPacketDescription	*inPacketDescriptions);
```
* inClientData 上下文对象；
* inumberOfBytes，读取的数据长度；
* inumberOfPackets，读取的数据帧数量；
* inInputData，读取的数据字节；
* AudioStreamPacketDescription类型的数组，存储了当前帧数据的偏移量和大小。

![图片来源：[Audio Streaming ( Audio Queue )](http://stevenkuo-blog.logdown.com/posts/303892-ios-audio-streaming-audio-queue)](http://upload-images.jianshu.io/upload_images/143845-2e0bb7471fb60b0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##五、Audio Queue Services
####5.1、初始化Audio Queue
```
AudioQueueNewOutput(                const AudioStreamBasicDescription *inFormat,
                                    AudioQueueOutputCallback        inCallbackProc,
                                    void * __nullable               inUserData,
                                    CFRunLoopRef __nullable         inCallbackRunLoop,
                                    CFStringRef __nullable          inCallbackRunLoopMode,
                                    UInt32                          inFlags,
                                    AudioQueueRef __nullable * __nonnull outAQ)  
```
 * AudioStreamBasicDescription音频数据格式类型，是一个AudioStreamBasicDescription对象，是使用AudioFileStream或者AudioFile解析出来的数据格式信息；
 * AudioQueueOutputCallback是某块Buffer被使用之后的回调；
 * inUserData 上下文对象；
 * inCallbackRunLoop为AudioQueueOutputCallback需要在的哪个RunLoop上被回调，如果传入NULL的话就会再AudioQueue的内部RunLoop中被回调，所以一般传NULL就可以了；
 * inCallbackRunLoopMode为RunLoop模式，如果传入NULL就相当于kCFRunLoopCommonModes，也传NULL就可以了；
 * inFlags是保留字段，目前没作用，传0；
 * 返回生成的AudioQueue实例；

####5.2、创建buffer
```
extern OSStatus
AudioQueueAllocateBuffer(           AudioQueueRef           inAQ,
                                    UInt32                  inBufferByteSize,
                                    AudioQueueBufferRef __nullable * __nonnull outBuffer) 
```

*  AudioQueueRef 创建的AudioQueue
* inBufferByteSize buffer的大小
* AudioQueueBufferRef 返回当前创建的buffer实例。

####5.3、将buffer放入音频队列
```
extern OSStatus
AudioQueueEnqueueBuffer(            AudioQueueRef                       inAQ,
                                    AudioQueueBufferRef                 inBuffer,
                                    UInt32                              inNumPacketDescs,
                                    const AudioStreamPacketDescription * __nullable inPacketDescs) 
```
* AudioQueueRef 创建的AudioQueue
* AudioQueueBufferRef buffer对象
*  AudioStreamPacketDescription数组的数量
*  AudioStreamPacketDescription数组的指针地址
