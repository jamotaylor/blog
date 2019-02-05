---
title: 'Serializing *Specified Fields in .NET'
date: 2019-02-04 15:08:00
categories: 
- Dev
tags: 
- c#
- serialization
---

So I had an issue at work this week with some of our serialized fields missing (and yes, I do spell serialized the American way).  Turns out it was a very sneaky little thing indeed...

As far as serialization goes in .NET, I'm used to using attributes to specify fields which should be ignored or serialized with special conditions (e.g. the *[System.Xml.Serialization.XmlIgnore]* attribute), I had no idea about this sneaky convention:

> When you have a property in a class (e.g. *Name*) and a corresponding **boolean** property that **ends with "Specified"** (e.g. *NameSpecified*), the first property **will not be serialized** unless the **\*Specified** property is set to **true**.

This works for both JSON.NET and the built in *System.Xml.Serialization.XmlSerializer*, and I'm sure there are a bunch of other libraries where the convention is used.

There is a post about this on [Stack Overflow](https://stackoverflow.com/questions/6711906/net-why-must-i-use-specified-property-to-force-serialization-is-there-a-way), but it didn't go into enough detail for me, and I wasn't sure whether this was an actual convention or something that was achieved through attributes or some other magic.

In order to test it out, I created a little... uh... test.

## The Serialization Tests

Let's start with a simple [POD](https://en.wikipedia.org/wiki/Passive_data_structure) class:

``` csharp
namespace jnt.Serialization
{
  public class Message
  {
    public string MessageType { get; set; }

    public bool MessageTypeSpecified { get; set; }

    public int MessageCode { get; set; }

    public bool MessageCodeSpecified { get; set; }
  }
}
```

As you can, see, no attributes, no Data Members or Data Contracts, nothing fancy - just plain old public properties.

Now to write some basic serialization and some tests - again, the serialization is standard, out-of-box functionality.

### XML

#### The XML serializer

``` csharp
using System.Text;
using System.Xml;

namespace jnt.Serialization
{
  public class XmlSerializer
  {
    public string Serialize<T>(T toSerialize)
    {
      var serializer = new System.Xml.Serialization.XmlSerializer(typeof(T));
      var sb = new StringBuilder();
      using (var xmlWriter = XmlWriter.Create(sb, new XmlWriterSettings { Indent = true }))
      {
        serializer.Serialize(xmlWriter, toSerialize);
      }
      return sb.ToString();
    }
  }
}
```

#### The XML serializer tests

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using jnt.Serialization;
using System;

namespace jnt.RandomTests
{
  [TestClass]
  public class XmlSerializer_Tests
  {
    [TestMethod]
    public void Test_Without_Specified()
    {
      var message = new Message
      {
        MessageCode = 1234567890,
        MessageType = "Save Our Souls"
      };
      Console.WriteLine(new XmlSerializer().Serialize(message));
    }

    [TestMethod]
    public void Test_With_Specified()
    {
      var message = new Message
      {
        MessageCode = 1234567890,
        MessageCodeSpecified = true,
        MessageType = "Save Our Souls",
        MessageTypeSpecified = true
      };
      Console.WriteLine(new XmlSerializer().Serialize(message));
    }
  }
}
```

#### XML result with *Specified properties *true*

``` xml
<?xml version="1.0" encoding="utf-16"?>
<Message xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <MessageType>Save Our Souls</MessageType>
  <MessageTypeSpecified>true</MessageTypeSpecified>
  <MessageCode>1234567890</MessageCode>
  <MessageCodeSpecified>true</MessageCodeSpecified>
</Message>
```

#### XML result with *Specified properties *false*

``` xml
<?xml version="1.0" encoding="utf-16"?>
<Message xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <MessageTypeSpecified>false</MessageTypeSpecified>
  <MessageCodeSpecified>false</MessageCodeSpecified>
</Message>
```

### JSON

#### The JSON serializer

``` csharp
using Newtonsoft.Json;

namespace jnt.Serialization
{
  public class JsonSerializer
  {
    public string Serialize(object toSerialize)
    {
      return JsonConvert.SerializeObject(toSerialize, Formatting.Indented);
    }
  }
}
```

#### JSON serializer tests

``` csharp
using System;
using jnt.Serialization;
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace jnt.RandomTests
{
  [TestClass]
  public class JsonSerializer_Tests
  {
    [TestMethod]
    public void Test_Without_Specified()
    {
      var message = new Message
      {
        MessageCode = 1234567890,
        MessageType = "Save Our Souls"
      };
      Console.WriteLine(new JsonSerializer().Serialize(message));
    }

    [TestMethod]
    public void Test_With_Specified()
    {
      var message = new Message
      {
        MessageCode = 1234567890,
        MessageCodeSpecified = true,
        MessageType = "Save Our Souls",
        MessageTypeSpecified = true
      };
      Console.WriteLine(new JsonSerializer().Serialize(message));
    }
  }
}
```

#### JSON result with *Specified properties set *true*

``` json
{
  "MessageType": "Save Our Souls",
  "MessageTypeSpecified": true,
  "MessageCode": 1234567890,
  "MessageCodeSpecified": true
}
```

#### JSON result with *Specified properties set *false*

``` json
{
  "MessageTypeSpecified": false,
  "MessageCodeSpecified": false
}
```

## Conclusion

So, this is definitely some sort of convention. I couldn't find anything in the MSDN docs about this, or anything in the [Json.NET docs](https://www.newtonsoft.com/json/help/html/Introduction.htm). Perhaps it is there *somewhere*...

Of course, if you wanted to use this feature, you would most likely hide the *Specified properties in your serialized output using an attribute.

Let me know in the comments below if you were aware of this and if you have any links to more info.