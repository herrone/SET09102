####How does this configuration work?
This configuration uses a .MAUI template which is a very new thing to use. It works to provide a platform where you can use a single codebase to target many platforms.

####How do I know my configuration works?
I know my configuration works as I am using the predescribed tools (Visual studio) as seen in Fig 1, and I have correctly cloned (Fig 2) and pulled the team repository onto my local machine (Fig 3). Furthermore, when I run the configuration after enabling developer mode tools, I get this screen up and running, with the hello world text (Fig 4). 

####Why was this configuration used? 
.NET Maui is a new tool, which will be used more and more in the future, so it's a good idea to get us as new Software Engineerings used to it now. Additionally, as it works accross a wide range of platforms, so wouldn't ostrastice anyone on the course with a Macbook or linux. It also has excellent integration with Visual Studio, another important tool for us to get to grips with, which is always helpful. 

####What other ones could have been used?
We could have used flutter as it has similar capabilities, but given the context of the project, it would be quite strange for any of us to know DART (flutter's programming language), which incidentally also has a steep learning curve. C# on the other hand, is widely known by our cohort, and as most of us will have used some language from the C/Java family, it should be easy for those who don't know it.

####Are there any limitations to this choice?
There are several potenital limitations to choosing .NET MAUI; 

1) Size : Due to .NET runtime, NET MAUI apps may have larger file sizes than fully native apps.

2) .NET MAUI sometimes has a higher overhead than other systems. 

3) Platform-specific code or extensions may need to be used in certain cases, which slightly defeats the aim of having a fully unified API for cross-platform development.  

![1](/images/downloadvs.png)
|:--:|
Fig 1

  ![2](/images/getclone.png)|
  |:--:|
  Fig 2 

  ![3](/images/setUpWorking.png)|
  |:--:|
  Fig 3 

  ![4](/images/workingbuild.png)|
  |:--:|
  Fig 4 