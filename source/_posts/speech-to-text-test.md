---
title: Yes, sir. That's what serialization is and that's why it's such an important concept because pecans serialize an object data from one machine to another joke about a database.
# title: Where's my pregnant? in the programming languages, but they are just in the world of the okay.
date: 2019-01-28 19:35:13
categories: 
- Dev
tags: 
- random
- c#
---

So in the process of starting this blog, I started recording some audio files while driving to and from work - the idea being to capture my thoughts and ideas while they're still fresh in my mind. 

Then, when it came to writing my first post, I went looking for software to **transcribe** my recordings.  To my dismay, there was nothing free out there that could transcribe from a recorded file - only from streaming speech - as in speaking into a microphone.

## No problem! I'm a programmer, I'll make my own transcriber!

Of course, by *"Make my own"*, I actually mean *"Go and find someone elses code that I can use"*...

First hit on nuget was [***Google.Cloud.Speech API***](https://www.nuget.org/packages/Google.Cloud.Speech.V1/1.1.0-beta03).  The API is pretty amazing - so simple from the C# side. Even the authentication was easy to set up and pretty much automated.  Get the beta, because it has the *punctuation* functionality and some other cool new stuff since V1.1.

> If you wanna test the API out just for fun - head to [this link](https://cloud.google.com/speech-to-text/) where you can upload files and see the results.  I tested it with these sounds from [Nacho Libre](https://www.soundboard.com/sb/nacho_libre_movie_quotes) - works a charm! 😁

If you want to skip all the cody bits, you can go straight to the [**results**](#the-results) of my *amazing* test.

To be able to use the API there are a couple of things you have to do.  I'm not running through these steps in any detail because the Google Cloud console UI is pretty easy to get around and there's also lots of [good documentation](https://cloud.google.com/docs/).

#### 1. Get a Google Account

#### 2. Start a [Google Cloud](https:\\cloud.google.com) free trial

+ You do have to provide Credit/Debit card details but Google swear that you won't be charged a cent until you manually activate billing. I trust them 🤷‍

#### 3. Create a Service Account in your Google Cloud Console

+ Give yourself the **owner** permission level so that you can do anything you want.

#### 4. Enable the Cloud.Speech API

#### 5. Add credentials to the Cloud.Speech API

#### 6. Download your credentials as a JSON file

#### 7. Create default credentials on your machine

Once you have your credentials, you need to create a environment variable that's points the the location of the JSON file which contains your keys. They do tell you how to do this on the [Google Cloud docs](https://cloud.google.com/docs/authentication/getting-started), but that didn't work for me on my Windows 10 machine.

Instead, hit the Start key and search for "environment" - then open up the Environment Variables UI. Under the section "User Variables for [user name]" click "New..."

{% asset_img google-cloud-enviro-window.png %}

Add a variable named "GOOGLE_APPLICATION_CREDENTIALS" then click the "Browse File..." button and browse to the JSON file you've just downloaded.

{% asset_img google-cloud-enviro-new.png %}

## The Code

Now that your default credentials are loaded on your machine, you can go ahead and start writing you Unit Tests. I created a **.NET Standard Class Library* and a *MSTest* project.  The code is really straightforward.

If you're trying to do recognition on a large file like mine with more than 1 minute of audio then you will need to upload the file to your cloud storage. [Read this](https://cloud.google.com/speech-to-text/quotas) for the allowed quotas, especially the line: *"Audio longer than ~1 minute must use the uri field to reference an audio file in Google Cloud Storage."*

As you can see in the code below, I had to use the *LongRunningRecognize* method because of the file length, and I'm using the *FromStorageUri* method to retrieve the file from my cloud storage.

Notice the *EnableAutomaticPunctuation* property in the *RecognitionConfig* is set *true* - as mentioned before, this is only available in the beta.

Now you have to match your audio file type to the *RecognitionConfig.Encoding* type, and there are only [some formats](https://cloud.google.com/speech-to-text/docs/encoding) that the API supports.  I chose .flac basically because it's first on the list.

I used [this service](https://audio.online-convert.com/convert-to-flac) to convert my MP3 to Flac and it worked really well.

{% asset_img convert-flac.png %}

Use the *Change the bit resolution* to set the bitrate to something compatible - either 16 or 24 bit. I have no idea what the sampling rate is for so I just chose 16000 Hz. Also, the API seems to only work with *mono* audio - so use the *Change audio channels* option there too.

``` csharp
using System;
using System.Text;
using Google.Cloud.Speech.V1;
using Google.LongRunning;
using static Google.Cloud.Speech.V1.RecognitionConfig.Types;

namespace SpeechRecordingToText
{
  public class SpeechToTextTest
  {
    public string LongSpeechRecognitionTest()
    {
      var audio = RecognitionAudio.FromStorageUri("gs://jnt-dev-speech-to-text/what is programming 2.flac");

      SpeechClient client = SpeechClient.Create();
      RecognitionConfig config = new RecognitionConfig
      {
        Encoding = AudioEncoding.Flac,
        SampleRateHertz = 16000,
        LanguageCode = LanguageCodes.English.UnitedStates,
        EnableAutomaticPunctuation = true,
      };

      var operation = client.LongRunningRecognize(config, audio);

      operation = operation.PollUntilCompleted();

      LongRunningRecognizeResponse response = operation.Result;

      var sb = new StringBuilder();
      foreach (var r in response.Results)
      {
        sb.Append(r.Alternatives[0].Transcript);
      }

      return sb.ToString();
    }
  }
}
```

#### Unit Test

**Make sure you that are running your unit tests (or IDE) as the user that you loaded the Environment Variable for, otherwise the automatic credentials won't work.**

```csharp

using System;
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace Tests_SpeechToText
{
  [TestClass]
  public class UnitTest1
  {
    [TestMethod]
    public void TestMethod2()
    {
      Console.WriteLine(new SpeechRecordingToText.SpeechToTextTest().LongSpeechRecognitionTest());
    }
  }
}
```

## The Results

The results were... well... not my best work... it reads like the system log from Douglas Adams' [Infinite Improbability Drive](https://hitchhikers.fandom.com/wiki/Infinite_Improbability_Drive):

> Okay where we were with object-oriented sterilization? Richard knows how to do because somebody is being very active and looking a bunch of codes. Reliable Parts of tick I'm not text with you have would have the the name of my objects and then the name of the different properties of math and the value. So you have every sort of of sterilization is broken down into That things can have a name an event. And then you can have a raised you can have collections of me. So if you would take that that's and you would converted into that she realized text and then the Peugeot I'm sending it to my friends program engine, which is considered as an object in that language. Where's my pregnant? in the programming languages, but they are just in the world of the okay. Yes, sir. That's what serialization is and that's why it's such an important concept because pecans serialize an object data from one machine to another joke about a database. It's a fairly similar will be used to seeing fall of data stored on. On your computer on your desktop simplest form of dates that you can really look at windows. all the equivalent in Mac You'll see the file extension. Txt. And you got some inside. What do bestie done GIF created a Text document that contains Unicode characters, which is that mapping. I was talking about earlier and each one of those characters is coming in 56. So 10 words would be one box. I-10 code four cases of be about 1 bucks best and that's a simple fall so you can that is called that is how it is stored in a file and it's cold and encoding to tell that day too hot. ohashi written and read in most cases it will be difficult to answer Unicode characters in a text file and now you can imagine that's if I've got a Word document when you open a Word document, that's because Microsoft Word Is there any how to open that kind of file She tried to open a Word document. with a notepad Doesn't know how to read a Word document. So you going to see a whole bunch of crazy characters that is trying to trying to convert binary data to text and it's saying what does value to text. It's out of the range of values that I'm expecting and so I'm just going to put this blockia. character use files full everything Play in memory on your hard drive. Quiznos the five star I'm all it does is read that file from beginning to end and that's how you end up with Violin Memory. Databases you might have heard of file to sterilize everything to so much more efficient in SQL Server. It breaks the dead up into very efficient little Cloud Supreme them together. That's basically like its own file system. and time is it? Can I get a full tank but just the first pick? Nike what the fastest what's the what's the difference? What's the where's the process? Okay. Okay, it's not a big difference. But if you just a fish stick images, it's not my car.

At least it was fun.  Looks like I'll just have to transcribe all those notes myself...  

Hey, on the plus side - this is kinda why we start with Unit Tests right?

It amazes me how easy to integrate this whole API was - and maybe I'll get a chance to come back to this sometime and play around some more... I've still got $300 to spend this year!