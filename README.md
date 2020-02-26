# Coding Music

Before I begin, I am in no way an expert in the topic, all I'll try to do is to provide a simple introduction to the topic of creating music from code. If you are looking for experts check out [TOPLAP](https://toplap.org/), a page dedicated to live coding music.

The follwing material will assume knowledge in programming, but only limited familarity with music theory and signal processing. 

(TODO requirement js console)

## Introductional Material

If you'd know nothing about coding music, [The Concert Programmer](https://www.youtube.com/watch?v=yY1FSsUV-8c) might be a good introduction to what we are going for. While there is a lot of material in the live coding scene, that is creating mostly experimental music, I got inspired by the idea of leveraging traditional music theory in combination with code to generate music mainly because I wanted to learn more about both.

For a great introduction into Music, check out [Learning Music by Ableton](https://learningmusic.ableton.com/).

## Hello World - Getting sound out of your computer

Without further ado lets produce some sound. What is the easiest way to produce sound from a computer in an automated way?

While I hope you might have guessed that it is something like `$ echo -e "\a"` which would let your mainboards speaker beep on some systems, this is not what I was going for.

Since (TODO year) webaudio api should be included in the major browsers. 

> Note: Before you execute the following code in your browser, please make sure you are set up to hear sound, but have set the volume to a low setting(!)

> Note: Don't get to attached to the browsers API as we'll move on to better ways of synthesizing sounds in a while

```JavaScript
ctx = new (window.AudioContext || window.webkitAudioContext)(); 
o = ctx.createOscillator();
o.connect(ctx.destination); 
o.start(); 
setTimeout(() => o.stop(), 400);
```

This should have produced a beep. (if not please check the error that was produced, or verify that your sound works).

Without installing a single piece of extra software we were able to produce sound in as little as 5 lines of code. (4 to be precise, but I included turning it off in the sample to be gentle with your earse).

This is still far away from music, and yet we are standing on the shoulders of giants here so lets take a moment to understand what we did.

1. We created a new audio context, this is based on the web audio api
1. We created an oscillator, so far this is just an object that does nothing but has useful properties and functions, that we'll use later
1. We connected the Output of that oscillator to the destination of our audio context. Because we have not made any modifications, this should be our standard sound output. This means the Oscillator is now ready to provide output.
1. We start the Oscillator. It now happily oscillates at it's default frequency 440 Hz.
1. Because neither do we want to listen to this Oscillator endlesly nor do we want it to stop instantly (we wouldn't hear anything), we are setting a timeout to stop the oscillator.

> Note: Check out [MDN - Oscillator Node](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode) if you want to learn how to change the behaviour of the Oscillator, like frequency or the wave form. If you want to know more about synths in general, visit [Ableton - Learning Synths](https://learningsynths.ableton.com/)

## Hello World #2 - More than beep

What if you want to get more out of your computer than a beep? Sound synthesis is a powerful tool and there is many ways to synthesize sounds in all forms and shapes. Yet, in many cases you might just want to play a prerecorded sound like [this example](TODO).

> Run `npm start` in this repo and browse to [localhost:8080](http://localhost:8080)

Easiest way: `new Audio('http://localhost:8080/beat.wav').play()` (TODO link)

> Note: Make sure you read the console output carefully to see if it complains about Autoplay being deactivated. This is a security measure. Click somewhere in the page and try again. 

> Note: See [MDN - Array Buffer](https://developer.mozilla.org/de/docs/Web/API/Body/arrayBuffer) for more information

The loop is a bit fast. The above method doesn't give us a lot of flexibility.

```JavaScript
audioCtx = new (window.AudioContext || window.webkitAudioContext)();

source = audioCtx.createBufferSource();
fetch("http://localhost:8080/beat.wav")
    .then(response => response.arrayBuffer())
    .then(buffer => audioCtx.decodeAudioData(buffer, (decoded) => {
        source.buffer = decoded;
        source.connect(audioCtx.destination);
    }))
    
source.start(0);
```

> Note: These examples are not following modern JavaScript coding practices. The code is supposed to run on a fresh page by copy pasting and use not to many new concepts.

Let's go through it step by step: 

1. Setting up the audio context 
1. Creating a new source node, an [MDN - Audio Buffer Source Node](https://developer.mozilla.org/en-US/docs/Web/API/AudioBufferSourceNode), just like we did with the Oscillator. Only that this source does not generate audio out of nothing, but instead plays back a stream of bytes. We have no bytes to play back yet.
1. Fething the audio file. This takes a while.
1. We want the Array buffer of that Response. Depending on the encoding of the file, this ByteArray might look completely different.
1. We decode the file format to get a usable Buffer of Bytes. The Audio Context provides the useful `decodeAudioData` function for this use case.
1. `source.start(0)` starts playback of that buffer on position 0, the beginning of the buffer. 

The Documentation for Audio Buffer Source Node shows some properties that look promising:

* `.loop` a boolean that allows us to specify if the Node should start playing the buffer again after it ended
* the samples `.playbackRate` - while loop can just be specified, playbackRate is of type [MDN - Audio Param](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam) and has to be used with a setter that also takes the time as argument to which the changes should happen

## Better Code & Temporal recursion 

Just playing one tone doesn't really make music, lets see how we could create a bass and play a simple melody. 

### Creating the Sound



### 

## Composing

A band has multiple instruments and we also might want to have different voices

## Timing 

While this is cool, audio is tricky, our ears are sensitive to timing

## Super Collider
