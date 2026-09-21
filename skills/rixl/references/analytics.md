# Analytics

Playback, engagement and traffic metrics. These operations are scoped by the caller's organization
rather than by `project_id`. Most take an item id plus a time range.

## Two range conventions

This is the most common source of argument errors.

| Endpoint | Range params |
|----------|--------------|
| `analytics.v1.DashboardService.GetDashboardStats` | `time_start` and `time_end`, both required, plus `interval` |
| everything else | `range.start` and `range.end`, both optional |

Both take RFC 3339 timestamps like `2026-01-15T10:30:00Z`. Send them as flat keys, so
`{"range.start": "..."}` and not a nested `range` object.

## Operations

| Operation | Params |
|-----------|--------|
| `analytics.v1.DashboardService.GetDashboardStats` | `time_start`, `time_end`, `interval`, `filter.countries`, `filter.cities`, `filter.devices`, `filter.os`, `filter.browsers`, `filter.languages`, `filter.regions`, `filter.os_versions` |
| `analytics.v1.DashboardService.ListDashboards` | `page_size`, `page` |
| `analytics.v1.DashboardService.GetDashboard` | `id` |
| `analytics.v1.DashboardService.ListDatasets` | none |
| `analytics.v1.VideosService.GetVideoStats` | `video_id`, `range.start`, `range.end` |
| `analytics.v1.ImagesService.GetImageStats` | `image_id`, `range.start`, `range.end` |
| `analytics.v1.PostsService.GetPostStats` | `post_id`, `range.start`, `range.end` |
| `analytics.v1.FeedsService.GetFeedStats` | `feed_id`, `range.start`, `range.end` |
| `analytics.v1.VideosService.GetTopVideos` | `limit`, `range.start`, `range.end` |
| `analytics.v1.ImagesService.GetTopImages` | `limit`, `range.start`, `range.end` |
| `analytics.v1.PostsService.GetTopPosts` | `limit`, `range.start`, `range.end`, `feed_id` |
| `analytics.v1.FeedsService.GetTopFeeds` | `limit`, `range.start`, `range.end` |
| `analytics.v1.HeatmapService.GetVideoHeatmap` | `video_id`, `buckets`, `range.start`, `range.end` |
| `analytics.v1.HeatmapService.GetHotSegments` | `video_id`, `range.start`, `range.end` |
| `analytics.v1.RealtimeService.GetRealtimeStats` | none |

## Notes

`ListDashboards` pages with `page_size` and `page`, not `pagination.limit` and `pagination.offset`.

`GetVideoHeatmap` returns engagement at each position in the video. `GetHotSegments` collapses the
same data into the segments that hold attention. Use hot segments for a summary and the heatmap when
the shape matters.

"What is my best video" is `GetTopVideos`, not a list followed by a sort.
