MediaToolkit
============

MediaToolkit is a .NET library which can convert and process both audio and video files.

This library is being updated almost daily , I will provide the full documentation by 13/Feb/2014. If you find any bugs or have any questions please let me know.

Contents
---------

1. [Functionalities](#functionalities)
2. [Get started!](#get-started)
3. [Samples](#samples)
4. [Licensing](#licensing)

Functionalities
-------------
- Convert video files into various other video formats.
- Create thumbnails from videos.
- Retrieve full media metadata.
- Perform video transcoding tasks.
    - Options configurable: `Bit rate`, `Frame rate`, `Resolution / size`, `Aspect ratio`, `Duration of video`
- Perform audio transcoding tasks.
    - Options configurable: `Audio sample rate`
- Convert video to physical formats using FILM, PAL or NTSC tv standards
    - Mediums include: `DVD`, `DV`, `DV50`, `VCD`, `SVCD`

Get started!
------------
Install our package from NuGets Package Manager Console using the following command

    PM> Install-Package MediaToolkit
    
[NuGet MediaToolkit](https://www.nuget.org/packages/MediaToolkit)

Samples
-------

- [Retrieve metadata](#retrieve-metadata)  
- [Perform basic video conversions](#basic-conversion)  
- [Grab thumbnail] (#grab-thumbnail-from-a-video)
- [Convert from FLV to DVD](#convert-flash-video-to-dvd)  
- [Convert FLV to MP4 using various transcoding options](#transcoding-options-flv-to-mp4)  
- [Cut / split video] (#cut-video-down-to-smaller-length)
- [Subscribing to events](#subscribe-to-events)

### Cut video down to smaller length

    var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};
    var outputFile = new MediaFile {Filename = @"C:\Path\To_Save_ExtractedVideo.flv"};

    using (var engine = new Engine())
    {
        engine.GetMetadata(inputFile);

        var options = new ConversionOptions();
        
        // This example will create a 25 second video, starting from the 
        // 30th second of the original video.
        //// First parameter requests the starting frame to cut the media from.
        //// Second parameter requests how long to cut the video.
        options.CutMedia(TimeSpan.FromSeconds(30), TimeSpan.FromSeconds(25));

        engine.Convert(inputFile, outputFile, options);
    }

### Grab thumbnail from a video

    var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};
    var outputFile = new MediaFile {Filename = @"C:\Path\To_Save_Image.jpg"};

    using (var engine = new Engine())
    {
        engine.GetMetadata(inputFile);
        
        // Saves the frame located on the 15th second of the video.
        var options = new ConversionOptions { Seek = TimeSpan.FromSeconds(15) };
        engine.GetThumbnail(inputFile, outputFile, options);
    }

### Retrieve metadata

    var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};

    using (var engine = new Engine())
    {
        engine.GetMetaData(inputFile);
    }
    
    Console.WriteLine(inputFile.Metadata.Duration);

### Basic conversion

    var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};
    var outputFile = new MediaFile {Filename = @"C:\Path\To_Save_New_Video.mp4"};

    using (var engine = new Engine())
    {
        engine.Convert(inputFile, outputFile);
    }

### Convert Flash video to DVD

    var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};
    var outputFile = new MediaFile {Filename = @"C:\Path\To_Save_New_DVD.vob"};

    var conversionOptions = new ConversionOptions
    {
        Target = Target.DVD, 
        TargetStandard = TargetStandard.PAL
    };

    using (var engine = new Engine())
    {
        engine.Convert(inputFile, outputFile, conversionOptions);
    }

### Transcoding options FLV to MP4

    var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};
    var outputFile = new MediaFile {Filename = @"C:\Path\To_Save_New_Video.mp4"};

    var conversionOptions = new ConversionOptions
    {
        MaxVideoDuration = TimeSpan.FromSeconds(30),
        VideoAspectRatio = VideoAspectRatio.R16_9,
        VideoSize = VideoSize.Hd1080,
        AudioSampleRate = AudioSampleRate.Hz44100
    };

    using (var engine = new Engine())
    {
        engine.Convert(inputFile, outputFile, conversionOptions);
    }

### Subscribe to events

    public void StartConverting()
    {
        var inputFile = new MediaFile {Filename = @"C:\Path\To_Video.flv"};
        var outputFile = new MediaFile {Filename = @"C:\Path\To_Save_New_Video.mp4"};
        
        using (var engine = new Engine())
        {
            engine.ConvertProgressEvent += ConvertProgressEvent;
            engine.ConversionCompleteEvent += engine_ConversionCompleteEvent;
            engine.Convert(inputFile, outputFile);
        }
    }

    private void ConvertProgressEvent(object sender, ConvertProgressEventArgs e)
    {
        Console.WriteLine("\n------------\nConverting...\n------------");
        Console.WriteLine("Bitrate: {0}", e.Bitrate);
        Console.WriteLine("Fps: {0}", e.Fps);
        Console.WriteLine("Frame: {0}", e.Frame);
        Console.WriteLine("ProcessedDuration: {0}", e.ProcessedDuration);
        Console.WriteLine("SizeKb: {0}", e.SizeKb);
        Console.WriteLine("TotalDuration: {0}\n", e.TotalDuration);
    }
    
    private void engine_ConversionCompleteEvent(object sender, ConversionCompleteEventArgs e)
    {
        Console.WriteLine("\n------------\nConversion complete!\n------------");
        Console.WriteLine("Bitrate: {0}", e.Bitrate);
        Console.WriteLine("Fps: {0}", e.Fps);
        Console.WriteLine("Frame: {0}", e.Frame);
        Console.WriteLine("ProcessedDuration: {0}", e.ProcessedDuration);
        Console.WriteLine("SizeKb: {0}", e.SizeKb);
        Console.WriteLine("TotalDuration: {0}\n", e.TotalDuration);
    }


Licensing
---------  
- MediaToolkit is licensed under the [MIT license](https://github.com/AydinAdn/MediaToolkit/blob/master/LICENSE.md)
- MediaToolkit uses [FFmpeg](http://ffmpeg.org), a multimedia framework which is licensed under the [LGPLv2.1 license](http://www.gnu.org/licenses/old-licenses/lgpl-2.1.html), its source can be downloaded from [here](https://github.com/AydinAdn/MediaToolkit/tree/master/FFMpeg%20src)

