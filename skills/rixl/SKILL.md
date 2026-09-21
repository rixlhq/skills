---
name: rixl
description: Read media, analytics, billing and content data from a Rixl organization through the Rixl MCP server. Use for questions about videos, images, feeds, posts, playback and engagement metrics, dashboards, invoices, usage, members, access policies and support tickets.
---

# Rixl

Rixl serves media, analytics and billing for organizations and their projects. This skill covers
reading that data through the Rixl MCP server.

Everything here is read-only. Every exposed operation is a GET, so nothing can create, change or
delete. If the user asks for a write, say so and point them at the dashboard or the REST API.

## The two tools

The whole API is served by two tools instead of one tool per endpoint, so endpoint names stay out of
context until they are needed.

| Tool | Use |
|------|-----|
| `search_endpoints(query, limit)` | Find an operation. Returns matches with `operation_id`, `method`, `path`, `tag` and a parameter list. |
| `call(operation, args)` | Run one. Pass an `operation_id` from a match as `operation` and its parameters as `args`. |

Search before calling. Operation ids cannot be guessed.

```
search_endpoints {"query": "list videos"}
  → {"operation": "ListVideos", "operation_id": "videos.v1.VideoService.ListVideos",
     "method": "GET", "path": "/media/v1/projects/{project_id}/videos",
     "params": [{"name": "project_id", "in": "path", "required": true}]}

call {"operation": "videos.v1.VideoService.ListVideos", "args": {"project_id": "QCQ82sukzY"}}
```

A short name like `ListVideos` also works as `operation` when only one operation has it.

## Rules

**Never build an id.** Project, feed, video, image, post and invoice ids are opaque. A guessed id is
either a not-found or someone else's object. Take every id from an earlier response.

**Read a project id first.** Most data hangs off a project. When a call needs `project_id` and you do
not have one, run `project.v1.ProjectService.ListProjects` and use an id from the result.

**Leave out `org_id`.** It defaults to the caller's own organization. Send it only when the user is
targeting a different org they have access to.

**Search with nouns.** Matching is on words, not meaning. A question works because filler is dropped,
but only the remaining nouns match. Common synonyms are understood, so "movies" finds videos and
"captions" finds subtitles. If a search returns nothing, try a different noun rather than a longer
sentence. An empty query returns every endpoint and is expensive.

**Fix errors in place.** An unknown operation comes back with the closest matches. A bad argument
comes back with the operation's parameter list. Read the error and correct the call instead of
starting again with a new search.

**Send parameters flat.** Names like `pagination.limit`, `range.start` and `filter.countries` are
literal keys. Do not nest them into objects.

## Where things live

Start here when you know the subject but not the endpoint. The middle column lists the `tag` values
that come back on a search match.

| Subject | Tags | Reference |
|---------|------|-----------|
| Projects, feeds, posts | Projects, Feeds, Posts | [content.md](references/content.md) |
| Videos, images, subtitles, chapters, audio tracks | Videos, Images, Subtitles, Chapters, Audio Tracks, Languages | [media.md](references/media.md) |
| Playback, engagement, dashboards, heatmaps, realtime | Dashboards, Video Analytics, Image Analytics, Post Analytics, Feed Analytics, Heatmaps, Realtime | [analytics.md](references/analytics.md) |
| Invoices, usage, plans, subscriptions, payments | Invoices, Usage, Plans, Subscriptions, Payments | [billing.md](references/billing.md) |
| Users, members, access policies, domains, API keys | Users, Memberships, Access Policies, Custom Domains, API Keys, Passkeys | [account.md](references/account.md) |
| Support tickets and their messages | Support Tickets, Support Chat | [support.md](references/support.md) |

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Calling without searching | Ids cannot be guessed, so run `search_endpoints` first |
| Building a `project_id` from the project name | Call `ListProjects` and take `id` from the response |
| Sending `org_id` on every call | Leave it out, it defaults to the caller's organization |
| Nesting `{"pagination": {"limit": 25}}` | Send `{"pagination.limit": 25}` |
| Searching again after an argument error | The error carries the parameter list, so fix the argument and re-call |
| Rewriting a failed search as a longer sentence | Try a different noun |
| Empty `query` to see everything | Expensive and floods context, so search the subject |
| Assuming a write exists | Read-only, there is no create, update or delete |
