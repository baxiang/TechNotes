>一、Audio Unit综述
　　1.1、Audio Unit 概念点
　　1.2、 AuidoUnit类型
二、构建Audio Unit的流程
　　2.1 、配置AudioSession
　　2.2、指定 Audio Units类型
　　2.3、创建AudioUnit
　　2.4、设置AudioUnit的属性
三、数据处理
　　3.1、 AURenderCallbackStruct
　　3.2、串连的Audio node
　　3.3、数据的转换
四、附录　
　　４.１、Audio Unit 示例

##一、Audio Unit综述
　　相对于MacOS,Audio Unit在iOS上使用到的几率很小，AV Foundation 和Audio Toolbox提供的API已经满足我们平常开发中音视频的录制播放的需求点。Audio Unit几乎可以认为是对硬件驱动层的封装，通过它获取麦克风采集的音频数据或者将音频数据传输给扬声器播放。但是随着直播热对音视频的传输速度高要求，将PCM音频转换成AAC主要用到就是Audio Unit。
![](http://upload-images.jianshu.io/upload_images/143845-06646768d469b800.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
　　与AV Foundation 和Audio Toolbox相比较，Audio Unit主要有两大优势:
（１）时效性高，Audio Unit是接近硬件层导致对音频流的采集回调更加迅速。 
（２）动态的配置，AUGraph可以动态的对音频数据的组合配置，改变音效。

####1.1Audio Unit 概念点：
　　Audio Unit 主要涉及到三个常用的概念知识：
（１）AUGraph：包含和管理Audio Unit 的组织者；
（２）AUNode /AudioComponent：是AUGraph音频处理环节中的一个节点。
（３）AudioUnit:  音频处理组件，是对音频处理节点的实例描述者和操控者。
　　我们不妨想像演唱会的舞台上，有录制歌声与乐器的麦克风，而从麦克风到输出到音响之间，还串接了大大小小的效果器，在这个过程中，无论是麦克风、音响或是效果器，都是不同的AUNode。AUNode 是这些器材的实体，而我们要操控这些器材、改变这些器材的效果属性，就会需要透过每个器材各自的操控界面，这些介面便是AudioUnit，最后构成整个舞台，便是AUGraph。AUNode 与AudioComponent 的差别在于，其实像上面讲到的各种器材，除了可以放在AUGraph 使用之外，也可以单独使用，比方说我们有台音响，我们除了把音响放在舞台上使用外，也可以单独拿这台音响输出音乐。当我们要在AUGraph 中使用某个器材，我们就要使用AUNode 这种形态，单独使用时，就使用AudioComponent。但无论是操作AUNode 或AudioComponent，都还是得透过AudioUnit 这一层操作界面。(上述文字摘自[KKBOX iOS/Mac OS X 基礎開發教材](https://zonble.gitbooks.io/kkbox-ios-dev/content/audio_apis/audio_api_overview.html))

　　下图所示两路音频数据首先经过均衡器单元，然后再经过混音单元组合在一起， 最后经由输入输出单元传输到到扬声器。
![](http://upload-images.jianshu.io/upload_images/143845-3895cdc7d69cd425.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####1.2 AuidoUnit类型
iOS提供了四大类别7种不同的AuidoUnit
AudioComponentDescription对象来描述一个具体的AudioUnit:
 ```
typedef struct AudioComponentDescription {
    OSType              componentType;
    OSType              componentSubType;
    OSType              componentManufacturer;
    UInt32              componentFlags;
    UInt32              componentFlagsMask;
} AudioComponentDescription;
```
* componentType AuidoUnit主要有四种大类型：均衡器/混音/输入输出/格式转换；
*  componentSubType 指的四大类型对应的子类型 可以对照下面的表；
* componentManufacturer 目前iOS开发中只有: kAudioUnitManufacturer_Apple；
* componentFlags和“componentFlagsMask一般设置为0。

|类型 |componentType | kAudioUnitType_Effect|
|:------:|:------:|:------:|
|均衡器|kAudioUnitType_Effect|kAudioUnitSubType_AUiPodEQ|
 |混音|kAudioUnitType_Mixer|kAudioUnitSubType_AU3DMixerEmbedded kAudioUnitSubType_MultiChannelMixer多路 混音
 | 输入输出|kAudioUnitType_Output|kAudioUnitSubType_RemoteIO远端kAudioUnitSubType_VoiceProcessingIO kAudioUnitSubType_GenericOutput|
|格式转换|kAudioUnitType_FormatConverter|kAudioUnitSubType_AUConverter|

##二、构建Audio Unit的流程
####2.1  配置AudioSession
```
AVAudioSession *audioSession = [AVAudioSession sharedInstance];
[audioSession setPreferredSampleRate:44100 error:&error];
[audioSession setPreferredInputNumberOfChannels:1 error:&error];
[audioSession setPreferredIOBufferDuration:0.05 error:&error];
[audioSession setActive: YES error: nil];
```
####2.2、指定 Audio Units类型
```
  // multichannel mixer unit
    AudioComponentDescription mixer_desc;
    mixer_desc.componentType = kAudioUnitType_Mixer;
    mixer_desc.componentSubType = kAudioUnitSubType_MultiChannelMixer;
    mixer_desc.componentManufacturer = kAudioUnitManufacturer_Apple;
    mixer_desc.componentFlags = 0;
    mixer_desc.componentFlagsMask = 0;
    // multichannel mixer unit
    AudioComponentDescription eq_desc;
    eq_desc.componentType = kAudioUnitType_Effect;
    eq_desc.componentSubType = kAudioUnitSubType_AUiPodEQ;
    eq_desc.componentManufacturer = kAudioUnitManufacturer_Apple;
    eq_desc.componentFlags = 0;
    eq_desc.componentFlagsMask = 0;
    // output unit
    AudioComponentDescription output_desc;
    output_desc.componentType = kAudioUnitType_Output;
    output_desc.componentSubType = kAudioUnitSubType_RemoteIO;
    output_desc.componentManufacturer = kAudioUnitManufacturer_Apple;
    output_desc.componentFlags = 0;
    output_desc.componentFlagsMask = 0;
```
####2.3、创建AudioUnit
（1）通过AudioComponent创建
```
    AudioComponent outComponent = AudioComponentFindNext(NULL,&output_desc);
    AudioUnit outUnit;
    AudioComponentInstanceNew(outComponent, &outUnit);
```
AudioComponentFindNext参数inComponent一般设置为NULL，从系统中找到第一个符合inDesc描述的Component，如果为其赋值，则从其之后进行寻找。
函数AudioComponentInstanceNew的第二个参数类型是AudioComponentInstance，AudioUnit实际上就是 AudioComponentInstance,在AudioComponent类中有定义：
```
typedef AudioComponentInstance AudioUnit;
```
（2）通过AUNode创建AudioUnit
AUGraph是由AUNode的串联而成，首先需要先创建一个 AUGraph:
```
  OSStatus status = NewAUGraph(&audioGraph);
```
获取一个 AUNode,第一个参数是创建的AUGraph。第二个参数是描述信息AudioComponentDescription，输出AUNode。
```
   AUNode mixerNode;
    status = AUGraphAddNode(audioGraph, &mixerUnitDescription, &mixerNode);
```

获取一个 AudioUnit,第一个和第三个参数是还是之前的AUGraph，AudioComponentDescription。第二个参数是我们刚才的AUNode，最终输出的AudioUnit。
```
 status = AUGraphNodeInfo(audioGraph, mixerNode, &mixerUnitDescription, &mixerUnit);
```

####2.4、设置AudioUnit的属性

![image.png](http://upload-images.jianshu.io/upload_images/143845-6c056d9b976ddd90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
AudioUnit实际上就是一个AudioComponentInstance实例对象，一个AudioUnit由scope(范围)和element(元素)组成，实际上开发中主要涉及到输入输出的问题
```
CF_ENUM(AudioUnitScope) {
	kAudioUnitScope_Global		= 0,
	kAudioUnitScope_Input		  = 1,
	kAudioUnitScope_Output		= 2,
	kAudioUnitScope_Group		= 3,
	kAudioUnitScope_Part		= 4,
	kAudioUnitScope_Note		= 5,
	kAudioUnitScope_Layer		= 6,
	kAudioUnitScope_LayerItem	= 7
};
```
scope主要使用到的输入kAudioUnitScope_Input和输出kAudioUnitScope_Output，而在element， Input用“1”（和I很像）表示，Output用“0”（和O很像）表示，

![image.png](http://upload-images.jianshu.io/upload_images/143845-1fac7507e7f02361.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
AudioUnitSetProperty(				AudioUnit				inUnit,
									AudioUnitPropertyID		inID,
									AudioUnitScope			inScope,
									AudioUnitElement		inElement,
									const void * __nullable	inData,
									UInt32					inDataSize)	
```

 *   AudioUnitPropertyID  设置属性名称
 *   AudioUnitScope  AudioUnit的Scope 主要用于输入输出范围
 *   AudioUnitElement AudioUnit的Element  主要用1 输入总线（bus）,0输出总线(bus);
 *   inData 输入值
 *   inDataSize 输入值的长度


   AudioUnit 的Remote IO有2个element，大部分代码和文献都用bus代替element，两者同义，bus0就是输出bus 1代表输入，播放音频文件就是在bus 0传送数据，bus 1输入在Remote IO 默认是关闭的，在录音的状态下 需要把bus 1设置成开启状态。
```
   UInt32 one = 1;
    AudioUnitSetProperty(outputUnit, kAudioOutputUnitProperty_EnableIO, kAudioUnitScope_Input, 1, &one, sizeof(one));
```
##三、数据处理
#### 3.1、AURenderCallbackStruct
在Audio Unit存储输入的数据和提供播放输出数据都是通过RenderCallback函数，通过AudioUnitSetProperty与输入输出回调相关联。
```
    AURenderCallbackStruct callBackStruct;
    callBackStruct.inputProc = BXAURenderCallback;
    callBackStruct.inputProcRefCon = (__bridge void * _Nullable)(self);
    AudioUnitSetProperty(_outAudioUinit, kAudioUnitProperty_SetRenderCallback, kAudioUnitScope_Global, 0, &callBackStruct, sizeof(AURenderCallbackStruct));
```
AURenderCallbackStruct是结构体，inputProc 就是我们注册回调函数。inputProcRefCon注册者。
```
typedef struct AURenderCallbackStruct {
	AURenderCallback __nullable	inputProc;
	void * __nullable			inputProcRefCon;
} AURenderCallbackStruct;

```
```
typedef OSStatus(*AURenderCallback)(void *							inRefCon,
						AudioUnitRenderActionFlags *	ioActionFlags,
						const AudioTimeStamp *			inTimeStamp,
						UInt32							inBusNumber,
						UInt32							inNumberFrames,
						AudioBufferList * __nullable	ioData);

```
* ioData 需要填充的缓存数据
* inNumberFrames 需要填充的数据帧数，根据这个帧数 从原始音频数据格式中输出多少frame的LPCM.
* ioActionFlags 数据回调发送错误或者其他情况 上下文传递数据。
*inBusNumber 是输出或者输出的哪个bus.


#### 3.2、串连的Audio node

![image.png](http://upload-images.jianshu.io/upload_images/143845-79d5456c7424a4d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
AUGraph 中的各个Audio unit 是传连接的 需要把各个unit 通过AUGraphConnectNodeInput 连接起来 一般主要用于混音 或者音效改变的时候 需要用到。
```
AUGraphConnectNodeInput(_audioGraph, EQNode, 0, outNode, 0);
```
#### 3.3、数据的转换
AudioConverterRef 第一个参数是输入的格式 第二个是需要输出的转换格式。
```
extern OSStatus
AudioConverterNew(      const AudioStreamBasicDescription * inSourceFormat,
                        const AudioStreamBasicDescription * inDestinationFormat,
                        AudioConverterRef __nullable * __nonnull outAudioConverter) 
```
需要把我们转换的LPCM格式回调输入AudioConverterFillComplexBuffer
```
extern OSStatus
AudioConverterFillComplexBuffer(    AudioConverterRef                   inAudioConverter,
                                    AudioConverterComplexInputDataProc  inInputDataProc,
                                    void * __nullable                   inInputDataProcUserData,
                                    UInt32 *                            ioOutputDataPacketSize,
                                    AudioBufferList *                   outOutputData,
                                    AudioStreamPacketDescription * __nullable outPacketDescription)
```
AudioConverterComplexInputDataProc回调函数就是读取原有数据的帧数据 放置于ioData中
```
static OSStatus BXPlayerConverterFiller(AudioConverterRef inAudioConverter, UInt32 * ioNumberDataPackets, AudioBufferList *ioData, AudioStreamPacketDescription** outDataPacketDescription, void * inUserData)
{
   
    BXAudioUnitEQPlayer *self = (__bridge BXAudioUnitEQPlayer *)(inUserData);
    
    if (self->_readPacketIndex >= self->_packetArray.count) {
        *ioNumberDataPackets = 0;
        return 'bxnd';
    }
    NSData *packet = self->_packetArray[self->_readPacketIndex];
    ioData->mNumberBuffers = 1;
    ioData->mBuffers[0].mData = (void *)packet.bytes;
    ioData->mBuffers[0].mDataByteSize = (UInt32)packet.length;
    
    static AudioStreamPacketDescription aspdesc;
    aspdesc.mDataByteSize = (UInt32)packet.length;
    aspdesc.mStartOffset = 0;
    aspdesc.mVariableFramesInPacket = 1;
    *outDataPacketDescription = &aspdesc;
    self->_readPacketIndex++;
     *ioNumberDataPackets = 1;
    return noErr;
}

```
## 四、附录
AudioUnit掌握需要同学们切合实际的敲代码运用[AudioUnit 使用的简单示例](https://github.com/baxiang/AudioUnit)
