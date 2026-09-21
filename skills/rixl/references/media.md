# Videos, images and their tracks

Listing media is project-scoped. Reading one item is id-scoped. Get a `project_id` from
[content.md](content.md) first.

## Operations

| Operation | Params |
|-----------|--------|
| `videos.v1.VideoService.ListVideos` | `project_id`, `pagination.limit`, `pagination.offset`, `sort_field`, `sort_direction` |
| `videos.v1.VideoService.GetVideo` | `video_id` |
| `images.v1.ImageService.ListImages` | `project_id`, `pagination.limit`, `pagination.offset`, `sort_field`, `sort_direction` |
| `images.v1.ImageService.GetImage` | `image_id` |
| `videos.v1.SubtitleService.ListSubtitles` | `video_id` |
| `videos.v1.ChapterService.GetVideoChapters` | `project_id`, `video_id` |
| `videos.v1.AudioTrackService.ListAudioTracks` | `video_id` |
| `videos.v1.LanguageService.ListLanguages` | none |

## Notes

`GetVideo` and `GetImage` take only the item id, with no `project_id`.

`GetVideoChapters` is the exception and needs both `project_id` and `video_id`.

Search synonyms work, so "movies" finds videos, "captions" finds subtitles and "thumbnails" finds
images.

To go from a video to its numbers, take the `video_id` into [analytics.md](analytics.md).
