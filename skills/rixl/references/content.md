# Projects, feeds and posts

Projects are the root. A project holds feeds, a feed holds posts, and media is listed per project.
Read the project id before anything else.

## Getting a project id

```
call {"operation": "project.v1.ProjectService.ListProjects"}
```

`org_id` is a path parameter here but defaults to the caller's organization, so leave it out. Take
`id` from the result and reuse it for every project-scoped call.

## Operations

| Operation | Params |
|-----------|--------|
| `project.v1.ProjectService.ListProjects` | none |
| `project.v1.ProjectService.GetProject` | `project_id` |
| `feeds.v1.FeedService.ListFeeds` | `project_id`, `pagination.limit`, `pagination.offset` |
| `feeds.v1.FeedService.GetFeed` | `project_id`, `feed_id` |
| `posts.v1.PostService.ListPosts` | `project_id`, `feed_id`, `pagination.limit`, `pagination.offset`, `creator_id` |
| `posts.v1.PostService.GetPost` | `project_id`, `post_id`, `feed_id` |

## Pagination

Page with the flat dotted names, not a nested object:

```
{"args": {"project_id": "...", "pagination.limit": 25, "pagination.offset": 50}}
```

## Duplicate post operations

Posts are reachable by several routes, so a search returns four list operations and three get
operations. They differ only in which ids sit in the path.

| Operation | Use when | Params |
|-----------|----------|--------|
| `posts.v1.PostService.ListPosts` | default | `project_id`, `feed_id`, `pagination.limit`, `pagination.offset`, `creator_id` |
| `posts.v1.PostService.ListPosts2` | filtering by creator | `project_id`, `feed_id`, `creator_id`, `pagination.limit`, `pagination.offset` |
| `posts.v1.PostService.ListPosts3` | you start from a feed id | `feed_id`, `project_id`, `pagination.limit`, `pagination.offset`, `creator_id` |
| `posts.v1.PostService.ListPosts4` | feed id plus creator | `feed_id`, `creator_id`, `project_id`, `pagination.limit`, `pagination.offset` |
| `posts.v1.PostService.GetPost` | default | `project_id`, `post_id`, `feed_id` |
| `posts.v1.PostService.GetPost2` | feed is known | `project_id`, `feed_id`, `post_id` |
| `posts.v1.PostService.GetPost3` | you start from a feed id | `feed_id`, `post_id`, `project_id` |

The difference is only which ids sit in the path. The 3 and 4 variants still accept `project_id`,
as a query parameter instead of a path one.

Prefer `ListPosts` and `GetPost`. The short names are ambiguous across this group, so always pass the
full `operation_id`.
