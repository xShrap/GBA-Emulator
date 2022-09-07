XAudioJS
A minimal cross-browser API for writing PCM audio samples:
This simple JavaScript library abstracts the push-for-audio API of Mozilla Audio, the MediaStream API in experimental Firefox Nightlies, and the passive callback API of Web Audio. This library introduces an abstraction layer that provides a push-for-audio and a callback API in one. We even provide a flash fallback to bring us to a total of 4 APIs supported.


This software is hereby placed in the public domain for anyone to use.
How To Initialize:
new XAudioServer(int channels, double sampleRate, int bufferLow, int bufferHigh, function underRunCallback, double volume, function failureCallback);
Make sure only one instance of XAudioServer is running at any time.
bufferLow MUST be less than bufferHigh.
bufferHigh sets the internal FIFO buffer length for all APIs except the Mozilla Audio Data API. Overfill on FIFO causes the oldest samples to be dropped first.
Array underRunCallback (int samplesRequested)
Arguments: Passed the number of samples that are needed to replenish the internal audio buffer back to bufferLow.

Functionality: JS developer set callback that can pass back any number of samples to replenish the audio buffer with.

Return: Array of samples to be passed into the underlying audio buffer. MUST be divisible by number of channels used (Whole frames required.). The return array length DOES NOT NEED to be of length samplesRequested.
volume is the output volume.
void failureCallback (void)
Functionality: JS developers set this callback to handle no audio support being available from the browser.
Function Reference:
void writeAudio (Array buffer)
Arguments: Pass an array of audio samples that is divisible by the number of audio channels utilized (buffer % channels == 0).
Functionality: Passes the audio samples directly into the underlying audio subsystem, and can call the specified sample buffer under-run callback as needed (Does the equivalent of executeCallback in addition to the forced sample input.).
Return: void (None).
void writeAudioNoCallback (Array buffer)
Arguments: Pass an array of audio samples that is divisible by the number of audio channels utilized (buffer % channels == 0).
Functionality: Passes the audio samples directly into the underlying audio subsystem.
Return: void (None).
int remainingBuffer (void)
Arguments: void (None).
Functionality: Returns the number of samples left in the audio system before running out of playable samples.
Return (On valid): int samples_remaining (CAN BE NEGATIVE)
Return (On invalid): null
void executeCallback (void)
Arguments: void (None).
Functionality: Executes the audio sample under-run callback if the samples remaining is below the set buffer low limit.
Return: void (None).
void changeVolume (double)
Arguments: double float between 0 and 1 specifying the volume.
Functionality: Changes the volume. Will affect samples in buffer, so has a low-latency effect (Use this to do a fast-mute).
Return: void (None).
