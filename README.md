![alt text](https://hackoween-timer.s3.amazonaws.com/Hackoween-2017-decal.png "Hack-o-Ween")


# Hack-o-Ween Bulq.com Podcast Alexa Skill

This skill will play the podcasts listed in audioAssets.js, currently episode 1 is being hosted on Soundcloud here: https://soundcloud.com/patrick-campbell-450277307 

## How it Works

Alexa Skills Kit now includes a set of output directives and input events that allow you to control the playback of audio files or streams.  There are a few important concepts to get familiar with:

* **AudioPlayer directives** are used by your skill to start and stop audio playback from content hosted at a publicly accessible secure URL.  You  send AudioPlayer directives in response to the intents you've configured for your skill, or new events you'll receive when a user controls their device with a dedicated controller (see PlaybackController events below).
* **PlaybackController events** are sent to your skill when a user selects play/next/prev/pause on dedicated hardware controls on the Alexa device, such as on the Amazon Tap or the Voice Remote for Amazon Echo and Echo Dot.  Your skill receives these events if your skill is currently controlling audio on the device (i.e., you were the last to send an AudioPlayer directive).
* **AudioPlayer events** are sent to your skill at key changes in the status of audio playback, such as when audio has begun playing, been stopped or has finished.  You can use them to track what's currently playing or queue up more content.  Unlike intents, when you receive an AudioPlayer event, you may only respond with appropriate AudioPlayer directives to control playback.

The sample project plays a pre-defined list of audio content defined in `js/audioAssets.js`, allowing the user to control playback with a range of custom and built-in intents.  It's organized into several modules:

* `index.js` is the main module that handles events from Alexa.  In the sample project, we setup the skill and register handlers defined in seperate modules.
* `constants.js` holds a few constants like the Application ID of the skill and the name of a table in DynamoDB the skill will use to store details about what each user has played.
* `audioAssets.js` is a list of audio content the skill will play from.
* `stateHandlers.js` is where the skill handles voice intent and playback control commands.  It registers handlers for each of the input events in different states the skill can be in, and defines a `controller` that centralizes the handler code since we perform the same action for several different input events (e.g., we do the same thing when the user tells the skill to stop or if the stop button is pressed on the device).  The sample project define three states:
    * **START_MODE** is the default state of the skill, such as when it's invoked without an intent for the first time.
    * **PLAY_MODE** is used when audio is currently being played by the skill.
    * **RESUME_DECISION_MODE** is used to handle the response from the user when they're asked to confirm they'd like to resume playback from a prior use of the skill.
* `audioEventHandlers.js` is where the skill handles AudioPlayer events.  These events are only expected in the PLAY_MODE state and are used to track the user's progress through the content. 

You can learn more about the new [AudioPlayer interface](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/custom-audioplayer-interface-reference) and [PlaybackController interface](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/custom-playbackcontroller-interface-reference).
