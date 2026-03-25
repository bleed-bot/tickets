# Tickets Guide
It covers:

- how to set the system up
- what every major setting does
- how panels, options, forms, timers, and transcripts work
- how to use ticket variables in message templates

## What The System Supports

- up to `15` ticket panels per server
- up to `5` active options per panel
- up to `5` standby options saved in the inactive mode of that panel
- up to `75` active options at once (5 * 15)
- up to `25` reusable forms per server
- button panels and dropdown panels
- custom greeting, claim, move, close, reopen, inactivity, and log messages
- optional greeting, close, and reopen DMs
- inactivity reminders
- auto-close timers
- per-option support roles
- blacklists
- transcripts
- reusable pre-open forms

## Quick Setup

1. Create a panel with `tickets panels`.
2. Configure the panel settings:
   - channel
   - mode
   - categories
   - panel message
   - max open tickets
   - log settings
3. Open `tickets options`. You will see one has been automatically created, of the type you picked.
4. Configure that option:
   - label
   - emoji
   - category override if wanted
   - support roles
   - greeting and lifecycle messages
   - timers
   - optional form
5. If you want intake questions, create a form with `tickets forms` and attach it to the option.
6. Use `tickets resend` if you want to recreate the panel message in a different channel.

## Command Overview

### Management Commands

```txt
tickets resend [channel] - Resend/recreate a live ticket panel message
tickets panels [name] - Manage ticket panels, or open a specific panel by name
tickets forms [name] - Manage reusable ticket forms, or open a specific form by name
tickets options - Manage ticket panel options
tickets blacklist [member|role] - View, add, or remove blacklist entries
tickets stats [member] - Show server ticket stats, or handled stats for a staff member
tickets list - List all currently open tickets in the server
```

### Ticket Action Commands

```txt
tickets allow [user|role] - Allow a user or role to see the current ticket
tickets allow list - List users and roles explicitly allowed to see the current ticket
tickets deny [user|role] - Remove a user or role from the current ticket
tickets unclaim [channel] - Remove the current claimer from a ticket
tickets rename [channel] (new name) - Rename a ticket channel
tickets move [channel] [reason] - Move an open ticket to another panel option
tickets close [channel] [reason] - Close a ticket
tickets delete [channel] [reason] - Delete a ticket and its channel
tickets reopen [channel] [reason] - Reopen a ticket
tickets claim [channel] [reason] - Claim a ticket
tickets transcript [channel] - Generate or refresh a ticket transcript
```

### Slash Commands

```txt
/allow add - Allow a user or role to see a ticket
/allow list - List users and roles explicitly allowed to see a ticket
/deny - Remove a user or role from a ticket
/unclaim - Remove the current claimer from a ticket
/rename - Rename a ticket channel
/tickets - List all currently open tickets in the server
/move - Move an open ticket to another panel option
/close - Close a ticket
/delete - Delete a ticket
/reopen - Reopen a ticket
/claim - Claim a ticket
/transcript - Generate or refresh a ticket transcript
```

## Panels

Panels are the public entry points users click to open tickets.

Each panel has these major settings:

- `Name`
  - internal name for management
  - must be unique per server
- `Channel`
  - where the live panel message is posted
  - can be changed by resending the panel
- `Mode`
  - `button` or `dropdown`
  - each panel keeps saved options for both modes
- `Panel Message`
  - the public message users click to open tickets
  - supports embeds and variables
- `Log Channel`
  - where ticket deletion logs and transcript logs are sent
- `Log Message`
  - custom log template used when logging ticket closures/deletions
- `Default Category`
  - primary category used for tickets opened on this panel
- `Overflow Category`
  - fallback category if the default category is full or unavailable
- `Maximum Open Tickets`
  - how many tickets one member may have open on this panel at once
- `Channel Name Format`
  - how ticket channels are named
  - built-in options:
    - case number
    - username
    - custom template
- `Case Padding`
  - pads case numbers for cleaner channel names
  - example: `7` becomes `0007`
- `Dropdown Placeholder`
  - only used when the panel is in dropdown mode
- `Auto-Pin Controls`
  - auto-pins the ticket control message
- `Claims Enabled`
  - controls whether staff can claim tickets
- `Delete After`
  - how long a deleted ticket waits before its channel is removed

## Options

Options are the individual ticket types under a panel.

A panel in button mode uses button options.
A panel in dropdown mode uses dropdown options.

### Shared Option Settings

- `Label`
  - visible option name
- `Emoji`
  - optional emoji shown on the option
- `Disabled`
  - prevents users from opening tickets with that option
- `Category`
  - optional per-option category override
  - falls back to panel categories if not set
- `Support Roles`
  - staff roles that should see or manage tickets from that option
  - if not set, the system falls back to global ticket staff roles
- `Form`
  - optional reusable form attached before ticket creation
- `Greeting Message`
  - message sent into the newly opened ticket
- `Greeting DM`
  - optional DM version of the greeting
- `Claim Message`
  - message sent when the ticket is claimed
- `Move Message`
  - message sent when the ticket is migrated to another option
- `Close Message`
  - message sent when the ticket is closed
- `Close DM`
  - optional DM version of the close message
- `Reopen Message`
  - message sent when the ticket is reopened
- `Reopen DM`
  - optional DM version of the reopen message
- `Inactivity Message`
  - reminder sent when the creator has gone quiet for too long
- `Inactivity Timer`
  - how long to wait before sending the inactivity reminder
- `Auto-Close Timer`
  - how long to wait before automatically closing the ticket

### Button-Only Settings

- `Style`
  - button color/style

### Dropdown-Only Settings

- `Description`
  - extra text shown under the dropdown label

## Forms

Forms are reusable intake flows that run before a ticket channel is created.

### Key Rules

- forms are owned at the server level
- a form can be attached to many options
- an option can have at most one form
- form answers are snapshotted onto the ticket
- old tickets keep the exact answers that were submitted at the time

### Supported Field Types

- `short_text`
- `long_text`
- `checkbox`
- `select`
- `role_select`
- `user_select`
- `channel_select`

### Form Limits

- up to `25` forms per server
- up to `5` fields per form
- up to `25` static options per select field

### Field Settings

All fields support:

- `Label`
- `Key`
- `Required`

Type-specific settings:

- `short_text`
  - description
  - placeholder
  - min length
  - max length
- `long_text`
  - description
  - placeholder
  - min length
  - max length
- `checkbox`
  - description
  - checked text
  - unchecked text
- `select`
  - description
  - placeholder
  - min values
  - max values
  - static child options
- `role_select`
  - description
  - placeholder
  - single-select
- `user_select`
  - description
  - placeholder
  - single-select
- `channel_select`
  - description
  - placeholder
  - single-select

## Blacklist And Access

- `tickets blacklist @member`
  - toggles a member on or off the ticket blacklist
- `tickets blacklist @role`
  - toggles a role on or off the ticket blacklist
- `tickets allow @member`
  - explicitly lets a user see the current ticket
- `tickets allow @role`
  - explicitly lets a role see the current ticket
- `tickets allow list`
  - shows the extra users and roles that were explicitly allowed into the current ticket
- `tickets deny @member`
  - removes a user's explicit ticket access
- `tickets deny @role`
  - removes a role's explicit ticket access

Blacklisted members or roles cannot open tickets.

Support visibility is controlled per option.
If an option does not define support roles, the system falls back to the global ticket staff role configuration.
Allow and deny are for extra ticket access on a specific ticket.
They do not replace the creator, claimer, or the ticket's built-in support/staff visibility rules.

## Ticket Lifecycle

### Open

When a ticket is opened:

- the selected panel and option are recorded
- the channel is created in the option category or panel category
- the creator gets access
- the panel/option context is saved for later messages, logs, and variables
- the greeting message can be sent
- timers begin if configured

### Claim

Claiming lets one staff member take ownership of a ticket.

- the claimer is recorded
- ticket visibility can be narrowed depending on configuration
- the claim message can be sent

### Unclaim

Unclaiming removes the current ticket owner.

- the claimer is cleared
- normal ticket visibility is restored
- the standard ticket controls come back

### Move

Moving a ticket migrates it to another option without opening a new channel.

- the panel/option context is updated
- the move reason is recorded
- the move message can be sent
- timers are recalculated against the new option

### Close

Closing a ticket:

- marks it closed
- hides the creator from the channel
- records who closed it and why
- can send a close message
- can send a close DM

### Reopen

Reopening a ticket:

- marks it open again
- restores the creator
- records who reopened it and why
- can send a reopen message
- can send a reopen DM

### Delete

Deleting a ticket:

- marks the ticket deleted
- records who deleted it and why
- can generate or update a transcript
- can log the final result in the configured log channel
- removes the channel after the configured delete delay

## Automation

### Inactivity Reminder

If configured on the option:

- the system tracks the creator's last message
- if they go quiet long enough, it sends the inactivity reminder
- if they reply again, the timer resets

### Auto-Close

If configured on the option:

- the system tracks the same creator activity window
- if the creator stays inactive long enough, the ticket closes automatically
- `ticket.closed_automatically` becomes available for templates

## Transcripts

The transcript system can:

- generate a transcript on demand
- refresh an existing transcript for the same ticket
- reuse the same transcript record instead of making endless duplicates
- include transcript links in logs and variables

Available access paths:

- `tickets transcript`
- `/transcript`

## Custom Messages And Embeds

The ticket system supports:

- plain message content
- embed blocks with `{embed}`
- message content inside embed mode with `$v{message: ...}`
- variables like `{ticket.case}`
- conditionals with `{if ...}{/if}` 

### Basic Embed Example

```txt
{embed}
$v{title: Ticket Intake}
$v{description: A new ticket has been opened.}
$v{field: Case && #{ticket.case} inline}
$v{field: Creator && {ticket.creator.full} inline}
$v{field: Option && {ticket.option.label} inline}
```

### Form Example

```txt
{embed}
$v{title: Ticket Intake}
$v{description: A new ticket was opened with the submitted form info.}
$v{field: User && {ticket.form.field.user} inline}
$v{field: Reason && {ticket.form.field.reason}}
```

### Auto-Close Example

```txt
{embed}
$v{title: Ticket Closed}
$v{description: This ticket has been closed.}

{if {ticket.closed_automatically} == yes}
$v{message: This ticket was closed automatically due to inactivity.}
{/if}

$v{field: Reason && {ticket.reason.closed}}
```

## Conditional Blocks

Supported syntax:

```txt
{if CONDITION}
content here
{/if}
```

Supported condition types:

- `{ticket.var} == value`
- `{ticket.var} != value`
- truthy checks such as `{if {ticket.form.id}}`

Current limits:

- no `{else}`
- no nesting

## Variable Basics

You can use variables:

- in plain message content
- in embed titles
- in embed descriptions
- in embed fields
- in footers and authors
- inside conditional blocks

Timestamps usually come in three forms:

- normal Discord timestamp
- `.raw` unix timestamp
- `.short` short date format

Example:

```txt
Opened: {ticket.opened_at}
Opened Raw: {ticket.opened_at.raw}
Opened Short: {ticket.opened_at.short}
```

## Variable Reference

```txt
TICKET CORE
ticket.case
ticket.id
ticket.open
ticket.deleted
ticket.opened_label
ticket.opened_at
ticket.opened_at.raw
ticket.opened_at.short
ticket.claimed_at
ticket.claimed_at.raw
ticket.claimed_at.short
ticket.migrated_at
ticket.migrated_at.raw
ticket.migrated_at.short
ticket.closed_at
ticket.closed_at.raw
ticket.closed_at.short
ticket.reopened_at
ticket.reopened_at.raw
ticket.reopened_at.short
ticket.deleted_at
ticket.deleted_at.raw
ticket.deleted_at.short
ticket.last_author_message_at
ticket.last_author_message_at.raw
ticket.last_inactivity_notice_at
ticket.last_inactivity_notice_at.raw
ticket.closed_automatically

TICKET REASONS
ticket.reason.claimed
ticket.reason.migrated
ticket.reason.closed
ticket.reason.reopened
ticket.reason.deleted

TICKET AUTHOR
ticket.author.id
ticket.author.name
ticket.author.display_name
ticket.author.tag
ticket.author.full

TICKET CREATOR
ticket.creator.id
ticket.creator.name
ticket.creator.display_name
ticket.creator.tag
ticket.creator.full

TICKET CLAIMER
ticket.claimer.id
ticket.claimer.name
ticket.claimer.display_name
ticket.claimer.tag
ticket.claimer.full

TICKET CLOSER
ticket.closer.id
ticket.closer.name
ticket.closer.display_name
ticket.closer.tag
ticket.closer.full

TICKET REOPENER
ticket.reopener.id
ticket.reopener.name
ticket.reopener.display_name
ticket.reopener.tag
ticket.reopener.full

TICKET DELETER
ticket.deleter.id
ticket.deleter.name
ticket.deleter.display_name
ticket.deleter.tag
ticket.deleter.full

TICKET CHANNEL
ticket.channel.id
ticket.channel.name
ticket.channel.mention

PANEL / CONTROLLER
ticket.panel.name
ticket.panel.mode
ticket.panel.message.id
ticket.panel.next_case_id
ticket.panel.dropdown_placeholder
ticket.panel.maximum
ticket.panel.channel.id
ticket.panel.channel.name
ticket.panel.channel.mention
ticket.panel.category.default.id
ticket.panel.category.default.name
ticket.panel.category.default.mention
ticket.panel.category.overflow.id
ticket.panel.category.overflow.name
ticket.panel.category.overflow.mention

ticket.controller.name
ticket.controller.mode
ticket.controller.message.id
ticket.controller.next_case_id
ticket.controller.dropdown_placeholder
ticket.controller.maximum
ticket.controller.channel.id
ticket.controller.channel.name
ticket.controller.channel.mention
ticket.controller.category.default.id
ticket.controller.category.default.name
ticket.controller.category.default.mention
ticket.controller.category.overflow.id
ticket.controller.category.overflow.name
ticket.controller.category.overflow.mention

OPTION
ticket.option.label
ticket.option.emoji
ticket.option.description
ticket.option.style
ticket.option.disabled
ticket.option.auto_close_after
ticket.option.auto_close_after.human
ticket.option.inactivity_time
ticket.option.inactivity_time.human
ticket.option.category.id
ticket.option.category.name
ticket.option.category.mention

The base timer variables above return raw seconds. Use the `.human` variants for formatted values like `5m`, `2h`, or `1d12h`.

FORM
ticket.form.id
ticket.form.name
ticket.form.title
ticket.form.submitted_at
ticket.form.submitted_at.raw
ticket.form.submitted_at.short

FORM FIELD PATTERN
ticket.form.field.<key>
ticket.form.field.<key>.raw
ticket.form.field.<key>.label
ticket.form.field.<key>.required
ticket.form.field.<key>.type

FORM FIELD ENTITY VALUES
user_select:
ticket.form.field.<key>.id
ticket.form.field.<key>.name
ticket.form.field.<key>.display_name
ticket.form.field.<key>.tag
ticket.form.field.<key>.full

role_select / channel_select:
ticket.form.field.<key>.id
ticket.form.field.<key>.name
ticket.form.field.<key>.mention

TRANSCRIPT
ticket.transcript.url
ticket.transcript.created_at
ticket.transcript.created_at.raw
```

## Tips

- Use `ticket.form.field.<key>` for the display value users submitted.
- Use `ticket.form.field.<key>.raw` when you need the raw stored value or selected ID.
- `ticket.creator.*` is an alias of `ticket.author.*`.
- Use `ticket.reason.closed`, `ticket.reason.reopened`, and the other reason vars for lifecycle messages.
- Use `ticket.closed_automatically` when you want auto-close specific wording.
- If you need a field to appear only sometimes, wrap it in `{if ...}{/if}`.
