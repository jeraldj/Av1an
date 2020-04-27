<h1 align="center">
    <br>
    Av1an
    </br>
</h1>

<h2 align="center">All-in-one tool for streamlining av1 encoding</h2>

![alt text](https://cdn.discordapp.com/attachments/665440744567472169/685103807952060447/143740_05_03_20.png)

<h4 align="center"> 
<a href="https://discord.gg/TssVH86"><img src="https://discordapp.com/api/guilds/696849974230515794/embed.png" alt="Discord server" /></a>
<img src="https://ci.appveyor.com/api/projects/status/cvweipdgphbjkkar?svg=true" alt="Project Badge"> <a href="https://codeclimate.com/github/master-of-zen/Av1an/maintainability"><img src="https://api.codeclimate.com/v1/badges/41ea7ad221dcdad3fe8d/maintainability" />
<img= src="https://app.codacy.com/manual/Grenight/Av1an?utm_source=github.com&utm_medium=referral&utm_content=master-of-zen/Av1an&utm_campaign=Badge_Grade_Dashboard"></a>
<a href="https://www.codacy.com/manual/Grenight/Av1an?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=master-of-zen/Av1an&amp;utm_campaign=Badge_Grade"><img src="https://api.codacy.com/project/badge/Grade/4632dbb2f6f34ad199142c01a3eb2aaf"/></a>
</h4>
<h2 align="center">Easy, Fast, and Efficient </h2>

Start using AV1 encoding. All open-source encoders are supported (Aom, Rav1e, SVT-AV1).

Example with default parameters:

    av1an -i input

With your own parameters:

    av1an -i input -enc aom -v " --cpu-used=3 --end-usage=q --cq-level=30 " -ff " -vf scale=-1:720 "
    -w 10 -p 2 -a " -c:a libopus -b:a 24k " -s scenes.csv -log my_log -o output_file

<h2 align="center">Usage</h2>

    -i   --file_path         Input file (relative or absolute path)

    -o   --output_file       Name/Path for output file (Default: (input file name)_av1.mkv)
                            Output file ending is always `.mkv`

    -enc --encoder          Encoder to use (aom,rav1e,svt_av1,vpx. Default: aom)
                            Example: -enc rav1e

    -v   --video_params     Encoder settings flags (If not set, will be used default parameters.
                            Required for SVT-AV1s)
                            Must be inside ' ' or " "

    -p   --passes           Set number of passes for encoding
                            (Default: Aomenc: 2, Rav1e: 1, SVT-AV1: 2)
                            At current moment 2nd pass Rav1e not working

    -a   --audio_params     FFmpeg audio settings flags (Default: copy audio from source to output)
                            Example: -a '-c:a libopus -b:a  64k'

    -w   --workers          Overrides automatically set number of workers.
                            Example: Rav1e settings " ... --tile-rows 2 --tile-cols 2 ... " -w 3

    -s   --scenes           Path to file with scenes timestamps.
                            If given `0` spliting will be ignored
                            If file not exist, new will be generated in current folder
                            First run to generate stamps, all next reuse it.
                            Example: "-s scenes.csv"

    -tr  --threshold        PySceneDetect threshold for scene detection Default: 50

    -ff  --ffmpeg             FFmpeg options. Applied to each segment individually 
                            Example:
                            --ff " -r 24 -vf scale=320:240 "

    -fmt --pix_format       Setting custom pixel/bit format(Default: 'yuv420p')
                            Example for 10 bit: 'yuv420p10le'
                            Encoding options should be adjusted accordingly.

    --resume                If encode was stopped/quit resumes encode with saving all progress
                            Resuming automatically skips scenedetection, audio encoding/copy,
                            spliting, so resuming only possible after actuall encoding is started.
                            /.temp folder must be presented for resume.

    --no_check              Skip checking numbers of frames for source and encoded chunks.
                            Needed if framerate changes to avoid console spam.
                            By default any differences in frames of encoded files will be reported.

    --keep                  Not deleting temprally folders after encode finished.

    -log --logging          Path to .log file(Default: no logging)

    --temp                  Set path for temporally folders. Default: .temp

    --boost                 Enable experimental CQ boosting for dark scenes. Refer to 1.7 release notes.

    -bl                     CQ limit for boosting. CQ can't get lower than this value.

    -br                     CQ range for boosting. Delta for which CQ can be changed
    
    --vmaf                  Calculate vmaf for each encoded clip.
                            Saves plot after encode, showing vmaf values for all video segments.
                            Requires: Installed FFMPEG with libvmaf and installed libvmaf.
                            
    --vmaf_path             Custom path to libvmaf models, by default used system one.
    
    --tg_vmaf               Vmaf value to target. Best works with 90-95 on 720/1080.

    --vmaf_steps            Number of evenly spaced probes that is used to interpolate vmaf to cq change.
                            Must be bigger than 3. Optimal is 4-6 probes. Default: 4
    
    --vmaf_error            Decrease initial Vmaf values for interpolation.Increasing number will result 
                            in lower CQ and bigger final vmaf score, use to correct whole vmaf plot. 
                            For start If target vmaf undershoot increase value by undershoot amount.
   
    --min_cq, --max_cq      Minimum and maximum CQ values used in interpolation.
                            Use to limit CQ values range. Default: 20, 63.

<h2 align="center">Main Features</h2>

**Spliting video by scenes for parallel encoding** because AV1 encoders currently not good at multithreading, encoding is limited to single or couple of threads at the same time.

*   [PySceneDetect](https://pyscenedetect.readthedocs.io/en/latest/) used for spliting video by scenes and running multiple encoders.
*   Fastest way to encode AV1 without lossing quality, as fast as many cores cpu have :).
*   Resuming encoding without loss of encoded progress.
*   Simple and clean console look.
*   Automatic determination of how many workers the host can handle.
*   Building encoding queue with bigger files first, minimizing waiting for last scene to encode.
*   Both video and audio transcoding with FFmpeg.
*   Logging of progress of all encoders.
*   "Boosting" quality of scenes based on their brightness.

## Install 
*   [Python3](https://www.python.org/downloads/) 
For Windows in installer set check option to `add Python to PATH`

*   [Av1an on PIP](https://pypi.org/project/Av1an/)
Installation: `pip install Av1an`

*   [FFmpeg](https://ffmpeg.org/download.html)
*   [AOMENC](https://aomedia.googlesource.com/aom/) For Aomenc encoder
*   [Rav1e](https://github.com/xiph/rav1e) For Rav1e encoder
*   [SVT-AV1](https://github.com/OpenVisualCloud/SVT-AV1) For SVT-AV1 encoder

### Donations for Threadripper 3990x dream
Bitcoin - 1gU9aQ2qqoQPuvop2jqC68JKZh5cyCivG
