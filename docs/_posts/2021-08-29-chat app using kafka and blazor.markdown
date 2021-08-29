---
layout: post
title:  "kChat - Chat application using Kafka and Blazor"
date:   2021-08-29 08:52:17 +1000
categories: kafka blazor
---
Recently I got involved in learning more about Kafka and as usual I got lost after watching a few vidoes. So I thought the best way to learn is to create a simple application.

### Why Chat ?

Chat application is a good candidate to send and receive messages via an event bus or service bus. It does not require more complex functionality like handling commits and eventually consistent. 

### Why Blazor ?

I love MVC and Blazor seems like the easiest way to create a real time streaming application on the web (via signal-r)

### Source Code

The source code can be found in github [here](https://github.com/TalalTayyab/kchat). The code is divided into different branches where each branch has code specific to a certain functionality.
This way the code can also be used in a workshop.

Branch|Description|
|-|-|
[Main](https://github.com/TalalTayyab/Kchat/tree/main)|Readme file that describes how to get a free Kafka instance
[First](https://github.com/TalalTayyab/Kchat/tree/first)|Layout of the Blazor app
[Second](https://github.com/TalalTayyab/Kchat/tree/second)|Add Kafka code
[Third](https://github.com/TalalTayyab/Kchat/tree/third)|Add web project into docker compose to simulate multiple client instances

## How to install Kafka

This section will discuss options on how to install Kafka.

### Locally using Docker-compose

Note: Requires Docker with support for linux containers.

1. Run `docker-compose up -d` in the `\kchat` folder and wait for it to complete.
1. Download the Kafka GUI [tool](https://kafkatool.com/download.html) aka Offset explorer
1. Open the Offset Explorer tool and Add a new connection `File -> Add New Connection`
1. Enter Cluster name example localhost - `Properties -> Cluster Name`
1. Enter bootstrap server `localhost:29092` (this is where Kafka is running) - `Advanced -> Bootstrap servers`
1. Press Test Connection and Yes to add connection.

### Free Cloud Kafka instance 

1. Browse to [CloudKafka](https://www.cloudkarafka.com/plans.html)
1. Scroll all the way down until you see Developer Duck Free account
1. Follow through the process to get a free instance of Kafka.

## Creating the blazor app
We are going to create a simple chat layout. First step is to create a new Blazor app using the `Blazor Server App` template. For more information on hosting models, read about those at [ASP.NET Core Blazor hosting models](https://docs.microsoft.com/en-us/aspnet/core/blazor/hosting-models?view=aspnetcore-5.0). In summary,

`With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app. UI updates, event handling, and JavaScript calls are handled over a SignalR connection.`

The structure of our app is simple. We will add a new razor component called `chat`. This is going to represent our page.

![kchat]({{ site.url }}/assets/kchat.png)

There is no interaction with Kafka at this stage. This is a bare bones Razor website. Notice the SendMessage code, it just appends the message to `_txtareaMessages` which is bound to the `<textarea>` control.

{% highlight C# %}
private void SendMessage()
{
    if (string.IsNullOrEmpty(_txtMessage))
    {
        return;
    }

    _txtareaMessages += _txtMessage + Environment.NewLine;
    _txtMessage = string.Empty;
}
{% endhighlight %}

**Tip**: Since Blazor establishes a connection over Signal-R you won't see normal HTML network traffic in developer tools. However if you enable WebSocket you can see the binary messages going between the server and client. See this stackoverflow [question](https://stackoverflow.com/questions/29771676/cant-see-signalr-traffic-in-browser-development-tools).

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
