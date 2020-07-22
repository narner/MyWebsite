---
title: "Push-To-Talk Audio Chat App"

---

Through December 2019 to Feburary 2020, [Julie Young](https://twitter.com/juliey4) and I built a push-to-talk audio chat app. It let users create audio channels that would be broadcast to others in a nearby physical location. 

Users could see available nearby channels and join them. Using the push-to-talk interface, they could talk to the channel.  



![PushToTalkLoginFlow](/blog_assets/2020/PushToTalkLoginFlow.png)



![PushToTalkMainStoryboard](/blog_assets/2020/PushToTalkMainStoryboard.png)

&nbsp;



**Creating Audio Channels**

We used the [OpenTok (now known as the Vonage Video API)](https://www.vonage.com/developer-center/?icmp=mainnav_developers_novalue) SDK for audio channel creation and communciation. A simple server was hosted on Heroku that allowed for interfacing between our app and the OpenTok API, allowing for easily creating new audio channels. 

We created audio channels using a combination of a randomly - generated number, and the user supplied channel name. We passed this to the OpenTok API, and used the token it gave back as a way to create a new audio channel that would then be broadcast to nearby phones:

```
self.createAudioChannel(readableChannelName: self.createdChannelReadableName, fullChannelName: self.createdChannelName, id: self.createdChannelSessionID, token: self.createdChannelToken, createdTime: createdTime)
```

&nbsp;

**Interacting with Audio Channels**

Whenever a user joined a new audio channel, three objects were created: 

```
var session: OTSession?
var subscriber: OTSubscriber?
var publisher: OTPublisher?
```

The `session` object would create an OpenTok session with an API key and `sessionID`.

The `subscriber` object handled listening to audio streams from a session (in other words, the audio of other participants in the audio channel).

Finally, the `publisher` would handle the broadcat of your own live audio to other members of the audio channel. 

&nbsp;

**Push To Talk** 

Push To Talk functionality was achieved by setting the `publishAudio` property of an instance of the OpenTok  `OTPublisher` object to true if the Microphone button was held down: 

```
@IBAction func pressButtonToTalk(_ sender: UIButton?) -> Void {
	userIsTalking = true
	publisher?.publishAudio = true
}
```

...and false if the button was released:

```
@IBAction func releaseButton(_ sender: UIButton?) -> Void {
	userIsTalking = false
	publisher?.publishAudio = false
}
```

&nbsp;

**Location-Based Broadcasting**

We used an open-soruce library called [MultiPeer](https://github.com/dingwilson/MultiPeer) to broadcast and receive channel information to nearby phones. MultiPeer is a wrapper around Apple's [MultipeerConnectivity Framework](https://developer.apple.com/documentation/multipeerconnectivity), and let's phones transmit information ot other phones that are nearby using Bluetooth and WiFi radios. 

When a user creates an audio channel, the channel name and ID is broadcast:

```
    @objc func broadcastNewChannel(){
        let broadcast: String = "NEW"+" "+self.createdChannelName + " " + self.createdChannelReadableName + " " + self.createdChannelSessionID
        MultiPeer.instance.send(object: broadcast, type: DataType.string.rawValue)
    }
```

This channel info would be broadcast on a loop, so that if a new phone entered the range of the space and they missed the initial broadcast that occured when the channel was created: 

```
self.broadcastTimer = Timer.scheduledTimer(timeInterval: 10, 
																					target: self, 
																					selector:#selector(self.broadcastNewChannel), 
																					userInfo: nil, 
																					repeats: true)
self.broadcastTimer.fire()
```

Additionally, we if the creator of a channel deleted the channel, that information would also be broadcast in the MultiPeer message, and any other nearby phones would remove that channel from their list of available channels to join. 

```
@objc func broadcastDeletedChannel(deletedChannel: String){
	let broadcast: String = "DELETE"+" "+deletedChannel+" " + "" + " " + ""
	MultiPeer.instance.send(object: broadcast, type: DataType.string.rawValue)
}
```



Any nearby phone that also had the app installed would listen for boradcast messages that included any available audio channels. 

```
func multiPeer(didReceiveData data: Data, ofType type: UInt32) {
	let receivedData = data.convert() as! String
	parseReceivedChannelData(dataString: receivedData)                       
}
```

The received data would then be parsed to determine the channel information, and create a new channel. These channels would populate a table view with the nearby channels that you could join: 

```
    func parseReceivedChannelData(dataString: String){
        let dataArray = dataString.split(separators: " ")
        let channelStatus = dataArray[0]
        let channelName = dataArray[1]
        let channelReadableName = dataArray[2]
        let channelID = dataArray[3]
        
        if channelStatus == "NEW" {
            if !availableFractalChannels.contains(where: { $0.fullChannelName == channelName}) {
                // channel does not exist, so add it
                _ = fetchChannelToken(channelName: channelName, channelReadableName: channelReadableName)
            } else {
                // channel exists already, don't add it again
            }
        }
        
        if channelStatus == "DELETE" {
            for f in availableFractalChannels {
                if f.fullChannelName == channelName {
                    availableFractalChannels.remove(object: f)
                    DispatchQueue.main.async() {
                        let section = self.tableViewAdaptor!.sections[0] as! TableViewAdaptorSection<ChannelTableViewCell, FractalChannel>
                        section.items = self.availableFractalChannels
                            self.tableViewAdaptor?.update()
                        self.tableViewAdaptor?.tableView.reloadData()
                    }
                }
            }
        }
    }
```



If you clicked on a channel, the app would navigate to a new screen with a button that would stream your microphone audio to the other members of the channel.

![JoinChannel+PushToTalk](/blog_assets/2020/JoinChannel+PushToTalk.gif)

