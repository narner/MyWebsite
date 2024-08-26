---
title: "Working with the Opus Audio Codec in Swift"
date: "2024-08-26"

---

The [Opus Interactive Audio Codec](https://opus-codec.org/) is an open-source, royalty free audio codec. It’s primarily used in applications like VoIP or voice-messaging applications, but can also be used for storage and streaming applications. It’s characterized by low latency, high quality, and wide platform support. 

In this blog post, we’ll go over how to setup an iOS app to get audio frames from the microphone, encode them using the Opus encoder, and then play them back using the Opus Decoder. 

Opus isn’t natively supported in Apple’s development frameworks, so we’ll use the [swift-opus package](https://github.com/alta/swift-opus) to help. It’s a set of Swift bindings over the C-based Opus codec. 

The code for this project can be found on GitHub here: https://github.com/narner/SwiftOpusAudioDemo



### Configuring the Encoder and Decoder

Our Opus Encoder/Decoder can be initialized a number of different ways depending on the goal of your app:

* VoIP (Voice over IP)
  * Optimized for real-time communications
  * Prioritizes low-latency and efficient speech compression
* Audio
  * Designed for general, non-speech audio; including music
  * Good for streaming music, podcasts, and audio book applications 
* Low Delay
  * Prioritizes low-latency over compression efficiency 
  * Used for live performances or interactive audio 

When setting up the encoder and decoder, you can configure which mode you want to use by setting the `application` to one of these three values: 

```
.voip
.audio
.restrictedLowDelay
```



You’ll need to ensure that you configure the encoder and decoder with a sample rate that is compatible with the Opus codec. Any one of these will be fine: 

* **8 kHz** (Narrowband)
* **12 kHz** (Medium-band)
* **16 kHz** (Wideband)
* **24 kHz** (Super-wideband)
* **48 kHz** (Fullband)



For this example, we’ll use the 48KHz and the VoIP configuration. We’ll configure these parameters inside of our `setupAudioSession` function, which also sets up the audio nodes for recording and playback:

```
private func setupAudioSession() {
	...
  let inputFormat = AVAudioFormat(standardFormatWithSampleRate: 	OPUS_ENCODER_SAMPLE_RATE, channels: 1)!
  let outputFormat = AVAudioFormat(standardFormatWithSampleRate: 	AUDIO_OUTPUT_SAMPLE_RATE, channels: AUDIO_OUTPUT_CHANNELS)!

  encoder = try Opus.Encoder(format: inputFormat, application: .voip)
  decoder = try Opus.Decoder(format: outputFormat, application: .voip)
}
```



### Encoding and Storing the Audio Buffers

Since this is a simple demo, we’re just going to immediately decode the Opus audio when we’re ready for playback, and play it back as raw PCM audio.

When we start recording, we install a tap on the audio input node, and extract the audio buffer: 

```
let inputFormat = AVAudioFormat(standardFormatWithSampleRate: OPUS_ENCODER_SAMPLE_RATE, channels: 1)!
let desiredBufferSize = AVAudioFrameCount((Double(OPUS_ENCODER_DURATION_MS) / 1000.0) * OPUS_ENCODER_SAMPLE_RATE)

inputNode.installTap(onBus: 0, bufferSize: desiredBufferSize, format: inputFormat) { [weak self] buffer, _ in
    self?.processBuffer(buffer)
}
```

And pass it into our `processBuffer` function, which encodes the raw PCM audio buffer as Opus data and adds it to our `encodedPackets` array:

```
private var encodedPackets: [Data] = []

private func processBuffer(_ buffer: AVAudioPCMBuffer) {
    guard let encoder = encoder else { return }

    do {
        var encodedData = Data(count: Int(buffer.frameLength) * MemoryLayout<Float32>.size)
        _ = try encoder.encode(buffer, to: &encodedData)
        encodedPackets.append(encodedData)
    } catch {
        print("Failed to encode buffer: \(error.localizedDescription)")
    }
}
```

Whenever the user is holding down the record button, we’ll get the raw audio data from the microphone and encode it as Opus data and store it for playback as soon as recording has finished. 



### Decoding and Playing Back the Audio Buffers

When we’ve finished recording and encoding the audio buffers as Opus data, we’ll then decode them back into raw audio and play the audio through the speaker. 

Inside of `stopRecordingAndPlay`, we’ll call `decodePackets` over each item in the array of encoded data packets. 

```
func stopRecordingAndPlay() {
    inputNode.removeTap(onBus: 0)
    isRecording = false
    statusMessage = "Processing and playing back..."

    let decodedBuffers = decodePackets()
    scheduleBuffersForPlayback(decodedBuffers)

    playerNode.play()
    isPlaying = true
}
```

```
private func decodePackets() -> [AVAudioPCMBuffer] {
  var decodedBuffers: [AVAudioPCMBuffer] = []

  for packet in encodedPackets {
      guard let decoder = decoder else { continue }
      do {
          let decodedBuffer = try decoder.decode(packet)
          decodedBuffers.append(decodedBuffer)
      } catch {
          print("Failed to decode packet: \(error.localizedDescription)")
      }
  }

  return decodedBuffers
}
```

 and then play back the decoded audio with `scheduleBuffersForPlayback`:

``` 
private func scheduleBuffersForPlayback(_ buffers: [AVAudioPCMBuffer]) {
    guard !buffers.isEmpty else {
        DispatchQueue.main.async {
            self.statusMessage = "No audio to play"
            self.isPlaying = false
        }
        return
    }

    for (index, buffer) in buffers.enumerated() {
        if index == buffers.count - 1 {
            playerNode.scheduleBuffer(buffer) {
                DispatchQueue.main.async {
                    self.statusMessage = "Playback complete"
                    self.isPlaying = false
                    self.encodedPackets.removeAll()
                }
            }
        } else {
            playerNode.scheduleBuffer(buffer)
        }
    }
}
```

### Putting it All Together 

[![](http://img.youtube.com/vi/GJW8DCIZoyE/0.jpg)](https://youtu.be/GJW8DCIZoyE"")



Now you know how to work with the Opus Codec in your Swift iOS or MacOS app. You can use this to build a low latency VoIP or audio-messaging based chat apps that will work over a variety of network bandwidths. 

In this example, we’ve gone over how to build an app that will take in raw audio, encode it via Opus; then decode it back to raw audio to play it back. However, you can also use Opus to encode and decode audio files, too. 





