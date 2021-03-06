# Audio-Spectrum-Analyzer

A series of Jupyter notebooks and python files which stream audio from a microphone using pyaudio.

Part 1 is a notebook which streams audio and displays the waveform with matplotlib.

Part 2 adds a spectrum viewer using scipy.fftpack to compute the FFT.


### Here is some additional information about the code you, or others, may find interesting:

FORMAT = pyaudio.paInt16

The line above means that every sample from the mic is of type INT16. Generically when you have type INT, that means that you have a number that can be plus or minus and it does NOT have a decimal point. For example: [-2, -56, -1, 0, 1, 5, 78] are all INTs, but -2.34 is not an integer. What the "16" means at the end of INT16 is that this particualar INT type only has 16 bits. having 16 bits means you can have 2^16 values... 2^16 is 65,536. Because you can have "+" or "-" values this means that half of the 65,536 are negative values and the other half are positive values (0 is included with the positive numbers). So that last thing to talk about here is the "Range" of the INT16 datatype. Range meaning from what number to what number does an INT16 cover. INT16 range is [-2^15 to 2^15-1] (the "-1" is because of 0 value)... so the range for an INT16 ends up being [-32768 to 32767].

This datatype of INT16 becomes important when we want to properly "read" the values given to us when we use the "stream.read(CHUNK)" function. All the "stream.read()" returns is binary data (1s and 0s). When we display it (use the print() function) Python shows us the values in HEX format one "Byte" at a time. A "Byte" is 8 bits (8 ones and zeros). This is why when the len() function is used on the stream.read() data it should double of the CHUNK value, because the len() function is returning how many BYTES of data not how many INT16s, but we know we asked for 16bits of data or 2 Bytes.  But really in the memory it just looks like this: [111010001000100010101010010101010100....].

We use the struct.unpack() function to tell Python how to read in all this ones and zeros. It takes two paramenters: struct.unpack("how many and what kind", "binary data"). We already know the "binary data" is the data we get from the stream.read() function. So what about the "how many and what kind", and by "kind" I mean datatype. We know we just pulled a "CHUNK" of data so that is the "how many", but what about the datatype... Well we specified it before, INT16. If you look at this website: https://docs.python.org/2/library/struct.html, it says that a signed integer of 2 bytes (16 bits) is coded with an 'h'. And because this parameter is needed in string format we convert CHUNK to a string and then add it in front of the 'h' (str(CHUNK) + 'h'). Let's say CHUNK is 4096, so the string value for the first parameter would be "4096h". Once again this means there are 4096 samples of audio that are INT16s in this binary data we received from the struct.unpack() function.
