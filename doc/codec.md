[Back to the README](../README.md#flutter-sound)

-------------------------------------------------------------------------------------------------------------------------------------

# Flutter Sound Codecs

## Actually, the following codecs are supported by flutter_sound:

|                   | AAC ADTS | Opus OGG | Opus CAF | MP3 | Vorbis OGG | PCM raw| PCM WAV | PCM AIFF | PCM CAF | FLAC    | AAC MP4 | AMR-NB | AMR-WB |
| :---------------- | :------: | :------: | :------: | :-: | :--------: | :----: | :-----: | :------: | :-----: | :-----: | :-----: | :----: | :----: |
| iOS encoder       | Yes      |   Yes(*) | Yes      | No  | No         | Yes    | Yes     | No       | Yes     | Yes     | Yes     | NO     | NO     |
| iOS decoder       | Yes      |   Yes(*) | Yes      | Yes | No         | Yes    | Yes     | Yes      | Yes     | Yes     | Yes     | NO     | NO     |
| Android encoder   | Yes      |   No     | No       | No  | No         | Yes    | Yes     | No       | No      | No      | Yes     | Yes    | Yes    |
| Android decoder   | Yes      |   Yes    | Yes(*)   | Yes | Yes        | Yes    | Yes     | Yes(*)   | Yes(*)  | Yes     | Yes     | Yes    | Yes    |

This table will eventually be upgrated when more codecs will be added.

Yes(*) : The codec is supported by Flutter Sound, but with a File Format Conversion. This has several drawbacks :
- Needs FFmpeg. FFmpeg is not included in the LITE flavor of Flutter Sound
- Can add some delay before Playing Back the file, or after stopping the recording. This delay can be substancial for very large records.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Raw PCM and Wave files

Raw PCM is not an audio format. Raw PCM files store the raw data without any envelope.
A simple way for playing a Raw PCM file, is to add a `Wave` header in front of the data before playing it. To do that, the helper verb `pcmToWave()` is convenient. You can also call directely the `startPlayer()` verb. If you do that, do not forget to provide the `sampleRate` parameter.

**A Wave file is just PCM data in a specific file format**.

The Wave audio file format has a terrible drawback : **it cannot be streamed**.
The Wave file is considered not valid, until it is closed. During the construction of the Wave file, it is considered as corrupted because he Wave header is still not written.


Note the following limitations in current the Flutter Sound version :
- The stream is  `PCM-Integer Linear 16` with just one channel. Actually, Flutter Sound does not manipulate Raw PCM with floating point PCM data nor with more than one audio channel.
- `FlutterSoundHelper duration()` does not work with Raw PCM file
- `startRecorder()` does not return the record duration.

-------------------------------------------------------------------------------------------------------------------------------------

## Recording or playing Raw PCM INT-Linerar 16 files

Please, remember that actually, Flutter Sound does not support Floating Point PCM data, nor records with more that one audio channel.

- To record a Raw PCM16 file, you use the regular `startRecorder()` API verb
- To play a Raw PCM16 file, you can either add a Wave header in front of the file with `pcm16ToWave()` verb, or call the regular `startPlayer()` API verb. If you do the later, you must provide the `sampleRate` parameter during the call.
You can look to the simple example provided with Flutter Sound.

*Example*
``` dart
var tempDir = await getTemporaryDirectory();
var fout = outputFile('${tempDir.path}/myFile.pcm');

await startRecorder
(
    codec: Codec.pcm16,
    toFile: outputFile,
    sampleRate: 16000,
    numChannels: 1,
);

await startPlayer
(
        fromURI: = outputFile,
        codec: Codec.pcm16,
        sampleRate: 16000, // Used only with codec == Codec.pcm16
        whenFinished: (){ /* Do something */},

);
```

-------------------------------------------------------------------------------------------------------------------------------------

## Recording PCM-16 to a Dart Stream

Please, remember that actually, Flutter Sound does not support Floating Point PCM data, nor records with more that one audio channel.

This works only with `openAudioSession()` and  does not work with `openAudioSessionWithUI()`.
You can look to the simple example provided with Flutter Sound.

Note : This new functionnality works better with Android minSdk >= 23, because previous SDK was not able to do UNBLOCKING `read`.

*Example*
``` dart
```

-------------------------------------------------------------------------------------------------------------------------------------

## Playing PCM-16 from a Dart Stream

Please, remember that actually, Flutter Sound does not support Floating Point PCM data, nor records with more that one audio channel.

This works only with `openAudioSession()` and does not work with `openAudioSessionWithUI()`.
You can look to the simple example provided with Flutter Sound.

Note : This new functionnality works better with Android minSdk >= 23, because previous SDK was not able to do UNBLOCKING `write`.

*Example*
``` dart
```

-------------------------------------------------------------------------------------------------------------------------------------

[Back to the README](../README.md#flutter-sound)
