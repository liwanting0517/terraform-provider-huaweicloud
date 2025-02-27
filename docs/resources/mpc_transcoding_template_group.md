---
subcategory: "Media Processing Center (MPC)"
---

# huaweicloud_mpc_transcoding_template_group

Manages an MPC transcoding template group resource within HuaweiCloud.

## Example Usage

```hcl
resource "huaweicloud_mpc_transcoding_template_group" "test" {
  name                  = "test"
  low_bitrate_hd        = true
  dash_segment_duration = 5
  hls_segment_duration  = 5
  output_format         = 1

  audio {
    bitrate       = 0
    channels      = 2
    codec         = 2
    output_policy = "transcode"
    sample_rate   = 1
  }

  video_common {
    max_consecutive_bframes = 7
    black_bar_removal       = 0
    codec                   = 2
    fps                     = 0
    level                   = 15
    max_iframes_interval    = 5
    output_policy           = "transcode"
    quality                 = 1
    profile                 = 4
  }

  videos {
    width   = 3840
    height  = 2160
    bitrate = 0
  }
}
```

## Argument Reference

The following arguments are supported:

* `region` - (Optional, String, ForceNew) The region in which to create the transcoding template group resource. If omitted,
  the provider-level region will be used. Changing this creates a new resource.

* `name` - (Required, String) Specifies the name of a transcoding template group.

* `output_format` - (Required, Int) Specifies the packaging type. Possible values are:
  + **1**: HLS
  + **2**: DASH
  + **3**: HLS+DASH
  + **4**: MP4
  + **5**: MP3
  + **6**: ADTS

-> If output_format is set to 5 or 6, do not set video parameters.

* `low_bitrate_hd` - (Optional, Bool) Specifies Whether to enable low bitrate HD. The default value is false.

* `hls_segment_duration` - (Optional, Int) Specifies the HLS segment duration. This parameter is used only
  when `output_format` is set to 1 or 3. The value ranges from 2 to 10. The default value is 5. The unit is second.

* `dash_segment_duration` - (Optional, Int) Specifies the dash segment duration. This parameter is used only when `output_format`
  is set to 2 or 3. The value ranges from 2 to 10. The default value is 5. The unit is second.

* `audio` - (Optional, List) Specifies the audio parameters. The [object](#audio_object) structure is documented below.

* `video_common` - (Optional, List) Specifies the video parameters.
  The [object](#video_common_object) structure is documented below.

* `videos` - (Optional, List) Specifies the list of output video configurations.
  The [object](#videos_object) structure is documented below.

<a name="audio_object"></a>
The `audio` block supports:

* `codec` - (Required, Int) Specifies the audio codec. Possible values are:
  + **1**: AAC
  + **2**: HEAAC1
  + **3**: HEAAC2
  + **4**: MP3

* `sample_rate` - (Required, Int) Specifies the audio sampling rate. Possible values are:
  + **1**: AUDIO_SAMPLE_AUTO
  + **2**: AUDIO_SAMPLE_22050 (22,050 Hz)
  + **3**: AUDIO_SAMPLE_32000 (32,000 Hz)
  + **4**: AUDIO_SAMPLE_44100 (44,100 Hz)
  + **5**: AUDIO_SAMPLE_48000 (48,000 Hz)
  + **6**: AUDIO_SAMPLE_96000 (96,000 Hz)

* `channels` - (Required, Int) Specifies the number of audio channels. Possible values are:
  + **1**: AUDIO_CHANNELS_1
  + **2**: AUDIO_CHANNELS_2
  + **6**: AUDIO_CHANNELS_5_1

* `bitrate` - (Optional, Int) Specifies the audio bitrate. The value is 0 or ranges from 8 to 1,000.
  The default value is 0. The unit is kbit/s.

* `output_policy` - (Optional, String) Specifies the output policy. Possible values are **discard** and **transcode**.
  The default value is transcode.

<a name="video_common_object"></a>
The `video_common` block supports:

* `output_policy` - (Optional, String) Specifies the output policy. Possible values are **discard** and **transcode**.
  The default value is transcode.

* `codec` - (Optional, Int) Specifies the video codec. Possible values are:
  + **1**: H.264
  + **2**: H.265

  The default value is 1.

* `profile` - (Optional, Int) Specifies the encoding profile. The recommended value is 3. Possible values are:
  + **1**: VIDEO_PROFILE_H264_BASE
  + **2**: VIDEO_PROFILE_H264_MAIN
  + **3**: VIDEO_PROFILE_H264_HIGH
  + **4**: VIDEO_PROFILE_H265_MAIN

  The default value is 3.

* `level` - (Optional, Int) Specifies the encoding level. Possible values are:
  + **1**: VIDEO_LEVEL_1_0
  + **2**: VIDEO_LEVEL_1_1
  + **3**: VIDEO_LEVEL_1_2
  + **4**: VIDEO_LEVEL_1_3
  + **5**: VIDEO_LEVEL_2_0
  + **6**: VIDEO_LEVEL_2_1
  + **7**: VIDEO_LEVEL_2_2
  + **8**: VIDEO_LEVEL_3_0
  + **9**: VIDEO_LEVEL_3_1
  + **10**: VIDEO_LEVEL_3_2
  + **11**: VIDEO_LEVEL_4_0
  + **12**: VIDEO_LEVEL_4_1
  + **13**: VIDEO_LEVEL_4_2
  + **14**: VIDEO_LEVEL_5_0
  + **15**: VIDEO_LEVEL_5_1

  The default value is 15.

* `quality` - (Optional, Int) Specifies the encoding quality. A larger value indicates higher encoding quality and
  longer transcoding time. Possible values are:
  + **1**: VIDEO_PRESET_HSPEED2
  + **2**: VIDEO_PRESET_HSPEED
  + **3**: VIDEO_PRESET_NORMAL

  The default value is 1.

* `max_iframes_interval` - (Optional, Int) Specifies the mximum I-frame interval. The value ranges from 2 to 10.
  The default value is 5. The unit is second.

* `max_consecutive_bframes` - (Optional, Int) Specifies the maximum number of B-frames.
  The vaule range is  0 to 7, and the default value is 4. The unit is frame.

* `fps` - (Optional, Int) Specifies the frame rate. Its value is 0 or an integer ranging from 5 to 30.
  The default value is 0. The unit is FPS.

* `black_bar_removal` - (Optional, Int) Specifies whether to enable black bar removal. Possible values are:
  + **0**: Disable black bar removal.
  + **1**: Enable black bar removal and low-complexity algorithms for long videos (>5 minutes).
  + **2**: Enable black bar removal and high-complexity algorithms for short videos (≤5 minutes).

  The default value is 0.

<a name="videos_object"></a>
The `videos` block supports:

* `width` - (Optional, Int) Specifies the video width. The value can be 0 or a multiple of 2 from 32 to 4,096 for H.264
  and 0 or a multiple of 4 from 160 to 4,096 for H.265. The unit is pixel. If this parameter is set to 0, the video width
  is an adaptive value. The default value is 0.

* `height` - (Optional, Int) Specifies the video height. The value is 0 or a multiple of 2 from 32 to 2,880 for H.264,
  and 0 or a multiple of 4 from 96 to 2,880 for H.265. The unit is pixel. If this parameter is set to 0, the video height
  is an adaptive value. The default value is 0.

* `bitrate` - (Optional, Int) Specifies the average output bitrate. The value is 0 or an integer ranging from 40 to
  30,000. The default value is 0. The unit is kbit/s. If this parameter is set to 0, the average output bitrate is an
  adaptive value.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - Indicates the transcoding template group ID.

* `template_ids` - Indicates the IDs of templates in the template group

## Import

MPC transcoding template groups can be imported using the `id`, e.g.

```
$ terraform import huaweicloud_mpc_transcoding_template_group.test 589e49809bb84447a759f6fa9aa19949
```
