# Support tickets and chat

Reading support history for the caller's organization.

## Operations

| Operation | Params |
|-----------|--------|
| `support.v1.SupportTicketService.ListTickets` | `pagination.limit`, `pagination.offset`, `status` |
| `support.v1.SupportTicketService.GetTicket` | `ticket_id` |
| `support.v1.SupportTicketService.ListCategories` | none |
| `support.v1.SupportTicketService.ListTopics` | `category_id`, `query` |
| `support.v1.SupportTicketService.ListTopics2` | `category_id`, `query` |
| `support.v1.SupportChatService.ListChatMessages` | `after`, `limit` |

## Notes

`ListTopics` and `ListTopics2` return the same thing and differ only in whether `category_id` sits in
the path. Use `ListTopics`, and pass the full `operation_id` because the short names are ambiguous.

`ListChatMessages` pages by cursor. Pass the last message id as `after` to get the next batch, rather
than an offset.

Tickets and chat contain whatever the customer wrote, which can include account details. Quote only
what answers the question.
