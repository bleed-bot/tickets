# Tickets Guide

It covers:

- how to set the system up
- what every major setting does
- how panels, options, forms, timers, and transcripts work
- how to use ticket variables in message templates

The current management UI is grouped by section:

- panels: `Behavior`, `Categories`, `Display`, `Messages`
- options: `Behavior`, `Form`, `Messages`, `Style`
- option `Behavior`: `Categories`, `Naming`, `Permissions`, `Button UX`
- option `Permissions`: `Member`, `Support`, `Trainees`
- option `Messages`: `Greeting`, `Required Roles`, `Claim`, `Move`, `Close`, `Reopen`, `Inactivity`, `Auto-Close`, `Auto-Delete`

## What The System Supports

- up to `15` ticket panels per server
- up to `5` active options per panel
- up to `5` standby options saved in the inactive mode of that panel
- up to `75` active options at once (5 * 15)
- up to `25` reusable forms per server
- button panels and dropdown panels
- custom greeting, required roles, claim, move, close, reopen, inactivity, auto-close, auto-delete, and log messages
- per-option claim, close, reopen, and delete button colors
- per-option claim, close, reopen, and delete reason popup toggles
- optional greeting, close, and reopen DMs
- inactivity reminders
- auto-close timers
- auto-delete after close timers per option
- close-on-leave automation per option
- per-option channel name format overrides
- per-option claim categories and claim rename templates
- per-option claim staff visibility toggles
- per-option close categories and close rename templates
- per-option ticket creator close toggles
- per-option support roles
- per-option trainee roles
- personal ticket claim profiles for support/staff/trainee members
- server-wide ticket profile enable/disable control
- per-option required-role gates with any-role or all-role matching
- per-option trainee claim, close, and speak toggles
- blacklists
- lifecycle-aware manual ticket access with `allow` / `deny`
- transcripts
- reusable pre-open forms

## Quick Setup

1. Create a panel with `tickets panels`.
2. Configure the panel settings:
   - channel
   - behavior
   - categories
   - display
   - panel message
   - log settings
3. Open `tickets options`. You will see one has been automatically created, of the type you picked.
4. Configure that option:
   - label
   - emoji
   - behavior
   - greeting and lifecycle messages
   - timers
   - optional form
5. If you want intake questions, create a form with `tickets forms` and attach it to the option.
6. Use `tickets resend` if you want to recreate the panel message in a different channel.

## Command Overview

`tickets` command aliases: `ticket`, `tix`

### Management Commands

```txt
tickets resend [channel] - Resend/recreate a live ticket panel message
tickets panels [name] - Manage ticket panels, or open a specific panel by name
tickets forms [name] - Manage reusable ticket forms, or open a specific form by name
tickets options - Manage ticket panel options
tickets profile - Manage your personal ticket claim profile
tickets profiles - Manage ticket profiles server wide as an admin
tickets blacklist [member|role] - View, add, or remove blacklist entries
tickets stats [30m|2h|4h20m|3d|2w|today|all] [member] - Show ticket stats for the server, or workload stats for a staff member
tickets list - List all currently open tickets in the server
```

### Ticket Action Commands

```txt
tickets allow [user|role] - Allow a user or role to view and send in the current ticket
tickets allow list - List users and roles explicitly allowed in the current ticket
tickets deny [user|role] - Remove a user or role from the current ticket
tickets unclaim [channel] - Remove the current claimer from a ticket
tickets rename [channel] (new name) - Rename a ticket channel, or rename any text channel if you have Manage Channels
tickets move [channel] [reason] - Move a ticket to another option, or move a regular text channel to a category if you have Manage Channels
tickets close [channel] [reason] - Close a ticket
tickets delete [channel] [reason] - Delete a ticket and its channel
tickets reopen [channel] [reason] - Reopen a ticket
tickets claim [channel] [reason] - Claim a ticket
tickets transcript [channel|case_id] - Generate or refresh a ticket transcript, or fetch an existing one by case ID
```

### Slash Commands

```txt
/allow add - Allow a user or role to view and send in a ticket
/allow list - List users and roles explicitly allowed in a ticket
/deny - Remove a user or role from a ticket
/unclaim - Remove the current claimer from a ticket
/rename - Rename a ticket channel, or rename any text channel if you have Manage Channels
/stats - Show ticket stats for the server or a staff member
/tickets - List all currently open tickets in the server
/move - Move a ticket to another option, or move a regular text channel to a category if you have Manage Channels
/close - Close a ticket
/delete - Delete a ticket
/reopen - Reopen a ticket
/claim - Claim a ticket
/transcript - Generate or refresh a ticket transcript, or fetch an existing one by case ID
```

## Permissions

### Management Commands

These require:

- effective `administrator`

Commands:

- `tickets resend`
- `tickets panels`
- `tickets forms`
- `tickets options`
- `tickets blacklist`
- `tickets profiles`

### Stats And Listing Commands

These require:

- one of:
  - effective `administrator`
  - a configured global ticket `staff role`
  - any configured ticket option `support role`

Commands:

- `tickets stats`
- `tickets list`
- `/tickets`

### Profiles

- `tickets profile`
  - available if you are configured as ticket support, ticket trainee, hold a global ticket staff role, or have effective `administrator`
- `tickets profiles`
  - requires effective `administrator`

### Claim

These require:

- claiming enabled on that panel/option
- the ticket must not already be claimed
- the ticket creator cannot claim their own ticket
- one of:
  - server owner / effective `administrator`
  - a configured option `support role`
  - a configured option `trainee role` with `Trainees Can Claim` enabled
  - a configured global ticket `staff role` when no support roles are set on that option

Commands:

- `tickets claim`
- `/claim`

### Close

These require:

- one of:
  - the ticket creator, if `Ticket Creator Can Close` is enabled on that option
  - the current claimer
  - server owner / effective `administrator`
  - a configured option `support role` if the ticket is not currently claimed
  - a configured option `trainee role` with `Trainees Can Close` enabled if the ticket is not currently claimed
  - a configured global ticket `staff role` if the ticket is not currently claimed and the option has no support roles

Commands:

- `tickets close`
- `/close`

### Delete / Reopen / Move / Transcript / Allow / Deny / Allow List / Unclaim

For ticket channels, these all share the same main access rule:

- one of:
  - the current claimer
  - server owner / effective `administrator`
  - a configured option `support role` if the ticket is not currently claimed
  - a configured global ticket `staff role` if the ticket is not currently claimed and the option has no support roles

Commands:

- `tickets delete`
- `/delete`
- `tickets reopen`
- `/reopen`
- `tickets move`
- `/move`
- `tickets transcript`
- `/transcript`
- `tickets allow`
- `tickets allow list`
- `/allow add`
- `/allow list`
- `tickets deny`
- `/deny`
- `tickets unclaim`
- `/unclaim`

Extra notes:

- `allow` and `deny` also require the bot to have `Manage Channels`.
- `delete` is stricter for trainees. A trainee cannot delete a ticket.
- `transcript` by case ID falls back to archived transcript access. If there is no matching live ticket record, only the server owner or an effective administrator can access it.

### Reason Editing

Reason editing uses its own access rule.

The following can update stored claim, close, reopen, and delete reasons:

- effective `administrator`
- the current claimer
- configured support or staff roles when the claimer is a trainee
- configured support or staff roles when the ticket is currently unclaimed

Commands:

- `tickets reason`
- `/reason`

### Rename

For ticket channels:

- same permissions as the shared ticket-action group above

For non-ticket channels:

- the actor must have effective `Manage Channels`

Commands:

- `tickets rename`
- `/rename`

### Move On Non-Ticket Channels

For non-ticket channels:

- the actor must have effective `Manage Channels`

Commands:

- `tickets move`
- `/move`

### Role Priority

When an option uses a mix of support roles, trainee roles, and global staff roles, the system resolves them in this order:

- `support`
- `trainee`
- `staff`

That means:

- if someone has both an option support role and an option trainee role, they are treated as `support`
- if someone has an option trainee role and also has a generic global staff role, they are treated as `trainee`
- generic staff roles do not override option-specific trainee behavior

## Panels

Panels are the public entry points users click to open tickets.

Panel management is grouped into these sections:

- `Behavior`
  - `Delete Delay`
    - how long a deleted ticket waits before its channel is removed
    - set it to `0s` if you want deletion to happen immediately
  - `Maximum Open Tickets`
    - how many tickets one member may have open on this panel at once
  - `Auto-Pin Controls`
    - auto-pins the ticket control message
  - `Claims Enabled`
    - controls whether staff can claim tickets
- `Categories`
  - `Default Category`
    - primary category used for tickets opened on this panel
  - `Overflow Category`
    - fallback category if the default category is full or unavailable
- `Display`
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
    - only shown and used when the panel is in dropdown mode
  - `Tickets Mode`
    - `button` or `dropdown`
    - each panel keeps saved options for both modes
- `Messages`
  - `Panel Message`
    - the public message users click to open tickets
    - supports embeds and variables
  - `Log Message`
    - custom log template used when logging ticket closures/deletions
    - also controls the log channel used for ticket logs
  - `Auto-Delete Message`
    - custom template sent when a closed ticket is scheduled for automatic deletion
    - only used when `Auto-Delete After Close` is enabled

Panel-level claim, close, reopen, and delete button styling is no longer configured here. Those button colors now live on the ticket option that opened the ticket.

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
- `Auto-Close Message`
  - optional message sent when this option closes a ticket automatically
- `Auto-Close Timer`
  - optional per-option timer that closes tickets after author inactivity
  - set it between `1m` and `30d`
- `Auto-Delete Message`
  - optional message sent when this option automatically deletes a closed ticket
- `Auto-Delete Timer`
  - optional per-option timer that deletes closed ticket channels automatically
  - set it to `0s` if you want the ticket to delete immediately after it is closed
  - reopening or manually deleting the ticket clears that pending auto-delete
- `Reopen Message`
  - message sent when the ticket is reopened
- `Reopen DM`
  - optional DM version of the reopen message
- `Inactivity Message`
  - reminder sent when the creator has gone quiet for too long
- `Inactivity Timer`
  - how long to wait before sending the inactivity reminder
- `Required Roles Message`
  - ephemeral denial message shown when a member fails this option's required-role check
  - edited from the option `Messages` section

Option behavior is grouped into these sections:

- `Categories`
  - `Category`
    - optional per-option category override
    - falls back to panel categories if not set
  - `Claim Category`
    - optional category override applied when the ticket is claimed
  - `Close Category`
    - optional category override applied when the ticket is closed
- `Naming`
  - `Channel Name Format`
    - optional per-option override for the panel channel naming format
  - `Claim Rename Template`
    - optional rename template applied when the ticket is claimed
  - `Close Rename Template`
    - optional rename template applied when the ticket is closed
- `Permissions`
  - `Member`
    - `Required Roles`
      - only members with these roles can open tickets from this option
      - if left empty, any member who can use the panel can open the option
    - `Require All Roles`
      - requires every selected role instead of allowing any one of them
      - only matters when at least one required role is configured
    - `Ticket Creator Can Close`
      - controls whether the ticket creator can close tickets from this option
    - `Close On Leave`
      - automatically closes open tickets from this option if the ticket creator leaves the server
      - uses the normal close flow and records the reason as `Ticket creator left the server.`
  - `Support`
    - `Support Roles`
      - staff roles that should see or manage tickets from that option
      - if not set, the system falls back to global ticket staff roles
    - `Keep Staff Visible On Claim`
      - lets option support roles keep seeing claimed tickets
      - if the option has no support roles, configured global ticket staff roles stay visible instead
      - disabled by default
    - `Staff Can Speak On Claim`
      - lets support roles that remain visible on claimed tickets continue replying there
      - only matters when `Keep Staff Visible On Claim` is enabled
  - `Trainees`
    - `Trainee Roles`
      - trainee roles that should always be able to see tickets from this option
    - `Trainees Can Claim`
      - allows configured trainees to claim tickets from this option
    - `Trainees Can Close`
      - allows configured trainees to close tickets from this option
    - `Trainees Can Speak`
      - allows configured trainees to reply in tickets from this option by default
- `Button UX`
  - each action has its own editor: `Claim`, `Close`, `Reopen`, `Delete`
  - each action can set:
    - button label
    - button emoji
    - button color
    - reason popup on or off
  - the `Delete` button still keeps its confirmation step even if its reason popup is disabled

### Button-Only Settings

- `Style`
  - button color/style

### Dropdown-Only Settings

- `Description`
  - extra text shown under the dropdown label

## Forms

Forms are reusable intake flows that run before a ticket channel is created.

### Form Settings

- `Name`
  - internal form name
- `Title`
  - visible modal title shown to users
- `Filtering Enabled`
  - whether server word filter and AutoMod keyword checks run against text answers before a ticket is opened

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
  - explicitly lets a user view and send messages in the current ticket while it is open
- `tickets allow @role`
  - explicitly lets a role view and send messages in the current ticket while it is open
- `tickets allow list`
  - shows the extra users and roles that were explicitly allowed into the current ticket
- `tickets deny @member`
  - removes a user's explicit ticket access
- `tickets deny @role`
  - removes a role's explicit ticket access

Blacklisted members or roles cannot open tickets.
If an option has `Required Roles` configured, those checks also run before the ticket is opened.

Support visibility is controlled per option.
If an option does not define support roles, the system falls back to the global ticket staff role configuration.
Allow and deny are for extra ticket access on a specific ticket.
They do not replace the creator, claimer, or the ticket's built-in support/staff visibility rules.
Manual allow entries are preserved through claim and unclaim, denied while the ticket is closed, and restored if the ticket is reopened.
Using `deny` removes that manual access entry entirely.

## Ticket Lifecycle

### Open

When a ticket is opened:

- the selected panel and option are recorded
- blacklist and required-role checks run before the channel is created
- the channel is created in the option category or panel category
- the creator gets access
- the panel/option context is saved for later messages, logs, and variables
- the greeting message can be sent
- timers begin if configured

### Claim

Claiming lets one staff member take ownership of a ticket.

- the claimer is recorded
- ticket visibility is reduced to the author, claimer, admins, and server owner by default
- if `Keep Staff Visible On Claim` is enabled on that option, the option support roles stay visible too
- if that option has no support roles, the server's global ticket staff roles stay visible instead
- manually allowed users and roles stay allowed while the ticket remains open
- if the option has a claim category, the ticket is moved there on claim
- if the option has a claim rename template, the ticket is renamed on claim
- the claim message can be sent

If the person claiming the ticket has a personal `tickets profile` configured, these claim-time settings can be overridden by their profile:

- claim category
- claim rename template
- claim message

Any profile field left blank falls back to the option's normal claim settings.

Administrators can use `tickets profiles` to:

- enable or disable ticket profiles server wide
- select a member from the user picker
- edit that member's profile overrides
- enable or disable that member's profile overrides

When server-wide profiles are disabled, saved personal profiles are kept but ignored until profiles are enabled again.

### Unclaim

Unclaiming removes the current ticket owner.

- the claimer is cleared
- normal ticket visibility is restored
- manually allowed users and roles stay allowed
- the standard ticket controls come back

### Move

Moving a ticket migrates it to another option without opening a new channel.

- the panel/option context is updated
- the current claimer is preserved if the ticket is already claimed
- the move reason is recorded
- the move message can be sent
- timers are recalculated against the new option

This applies to command-based moves and drag-based moves. If a ticket is moved with `tickets move`, `/move`, or a supported category drag, it keeps its current claim status.

If an **open** ticket channel is dragged into a category that uniquely matches either:

- a ticket option's category
- a panel's default category
- a panel's overflow category

the bot will try to treat that as a move automatically.

If a claimed ticket is dragged somewhere that does not resolve to a unique move target, no automatic move is performed and the existing claim is kept.

If a ticket is dragged into **Uncategorized**, no automatic move is performed. The channel stays uncategorized. If the ticket was claimed, it stays claimed there too.

When using `tickets move` or `/move` on a non-ticket channel, you must have **Manage Channels**. For `/move`, provide the `category` parameter to pick the destination from Discord's category selector.

### Close

Closing a ticket:

- marks it closed
- hides the creator from the channel
- also denies manually allowed users and roles while the ticket is closed
- if the option has a close category, the ticket is moved there on close
- if the option has a close rename template, the ticket is renamed on close
- records who closed it and why
- can send a close message
- can send a close DM

### Reopen

Reopening a ticket:

- marks it open again
- restores the creator
- restores manually allowed users and roles that were hidden on close
- records who reopened it and why
- can send a reopen message
- can send a reopen DM

### Delete

Deleting a ticket:

- marks the ticket deleted
- records who deleted it and why
- generates or refreshes a transcript before deletion, even if no log channel is configured
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
- the auto-close message is sent when that timer triggers
- `ticket.closed_automatically` becomes available for templates

### Auto-Delete After Close

If configured on the option:

- once a ticket is closed, a delete timer starts
- the auto-delete message is sent when that timer starts
- reopening the ticket clears that timer
- deleting the ticket manually clears that timer too
- when the timer expires, the closed ticket channel is deleted automatically

### Close On Leave

If `Close On Leave` is enabled on the option:

- any still-open ticket from that option closes automatically when the ticket creator leaves the server
- it uses the normal close flow instead of a special one-off shutdown path
- close category moves, close rename templates, close messages, and close DMs still behave normally
- the recorded close reason becomes `Ticket creator left the server.`

## Transcripts

The transcript system can:

- generate a transcript on demand
- generate or refresh a transcript automatically when a ticket is closed
- generate or refresh a transcript automatically when a ticket is deleted, even without a log channel
- refresh an existing transcript for the same ticket
- reuse the same transcript record instead of making endless duplicates
- include transcript links in logs and variables
- include transcript expiration timestamps in variables and the default transcript log
- fetch an existing saved transcript by case ID

Available access paths:

- `tickets transcript`
- `tickets transcript 123`
- `/transcript`
- `/transcript case_id:123`

## Custom Messages And Embeds

The ticket system supports:

- plain message content
- embed blocks with `{embed}`
- message content inside embed mode with `$v{message: ...}`
- variables like `{ticket.case}`
- conditionals with `{if ...}{/if}` 

This includes panel messages, option lifecycle messages, option required-role denial messages, option auto-close and auto-delete messages, and log messages.

When you leave a message field blank, the system falls back to bleed's built-in default code.
The default code shown in the editor matches the actual default message that gets sent.

### Required Roles Messages

Each option has its own `Required Roles` message in the option `Messages` section.

Use it when you want a custom response for members who are not allowed to open that option.

Important behavior:

- it is always sent ephemerally (invisibly)
- it is only shown when the member fails the option's required-role check
- role mentions and `@everyone` are suppressed there
- only the clicking member can be pinged
- it works especially well with the `ticket.option.required_roles.*` variables listed below

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

## Conditional Blocks

Conditional blocks can be used to change which content is displayed in a message, based on a given value (condition). Conditionals are a fundamental concept of programming that control the flow of a program.

With conditionals, you can essentially turn your bleed ticket embeds into a program, that is able to change values based on given inputs.

Supported syntax:

```txt
{if CONDITION}
code here that displays if condition is met
{elseif OTHER_CONDITION}
other code here that displays if elseif condition is met
{else}
other code here if nothing else matches
{/if}
```

### Auto-Close Example using if/else condition

```txt
{embed}
$v{title: Ticket Closed}
$v{description: This ticket has been closed.}

{if {ticket.closed_automatically} == yes}
$v{message: This ticket was closed automatically due to inactivity.}
{else}
$v{message: This ticket was closed by staff.}
{/if}

$v{field: Reason && {ticket.reason.closed}}
```


Supported condition types:

- `{ticket.var} == value`
- `{ticket.var} != value`
- truthy checks such as `{if {ticket.form.id}}`
  
`{elseif ...}` also supports the same condition formats. You can also write it as `{else if ...}`.

Current limits:

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
ticket.auto_delete_after_close
ticket.auto_delete_after_close.human
ticket.auto_delete_at
ticket.auto_delete_at.raw
ticket.auto_delete_at.short
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

TICKET USERS
These same user fields are available for each of:
ticket.author
ticket.creator
ticket.claimer
ticket.closer
ticket.reopener
ticket.deleter

Use any of the following suffixes with the prefixes above:
.id
.name
.display_name
.tag
.full
.mention
.bot
.display_avatar
.avatar
.guild_avatar
.created_at
.created_at_timestamp
.joined_at
.joined_at_timestamp
.color
.role_list
.role_text_list
.top_role
.role_count
.join_position
.join_position_suffix
.boost
.boost_since
.boost_since_timestamp

Member-specific fields like `.joined_at`, `.guild_avatar`, `.role_list`, `.top_role`, `.join_position`, and boost values return `N/A` when that data is not available for that ticket user.

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
ticket.panel.auto_delete_after_close
ticket.panel.auto_delete_after_close.human
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
ticket.controller.auto_delete_after_close
ticket.controller.auto_delete_after_close.human
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
ticket.option.auto_delete_after_close
ticket.option.auto_delete_after_close.human
ticket.option.inactivity_time
ticket.option.inactivity_time.human
ticket.option.category.id
ticket.option.category.name
ticket.option.category.mention
ticket.option.required_roles
ticket.option.required_roles.names
ticket.option.required_roles.mentions
ticket.option.required_roles.count
ticket.option.required_roles.mode
ticket.option.required_roles.match_all

The base timer variables above return raw seconds. Use the `.human` variants for formatted values like `5m`, `2h`, or `1d12h`. `ticket.auto_delete_at` uses the ticket's closed time plus the option auto-delete timer.

`ticket.option.required_roles.mode` returns `Any` or `All`.
`ticket.option.required_roles.match_all` returns `yes` or `no`.

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
For `user_select`, the field supports the same user suffixes as the main ticket user vars above.

Examples:
ticket.form.field.<key>.id
ticket.form.field.<key>.name
ticket.form.field.<key>.display_name
ticket.form.field.<key>.tag
ticket.form.field.<key>.full
ticket.form.field.<key>.mention
ticket.form.field.<key>.display_avatar
ticket.form.field.<key>.joined_at
ticket.form.field.<key>.role_list
ticket.form.field.<key>.top_role
ticket.form.field.<key>.boost_since

role_select / channel_select:
ticket.form.field.<key>.id
ticket.form.field.<key>.name
ticket.form.field.<key>.mention

TRANSCRIPT
ticket.transcript.url
ticket.transcript.created_at
ticket.transcript.created_at.raw
ticket.transcript.created_at.short
ticket.transcript.expires_at
ticket.transcript.expires_at.raw
ticket.transcript.expires_at.short
```

## Tips

- Use `ticket.form.field.<key>` for the display value users submitted.
- Use `ticket.form.field.<key>.raw` when you need the raw stored value or selected ID.
- `ticket.creator.*` is an alias of `ticket.author.*`.
- Use `ticket.reason.closed`, `ticket.reason.reopened`, and the other reason vars for lifecycle messages.
- Use `ticket.closed_automatically` when you want auto-close specific wording.
- Use `ticket.option.required_roles.mentions` and `ticket.option.required_roles.mode` in the `Required Roles` message when you want the denial to explain exactly what the member is missing.
- If you need a field to appear only sometimes, wrap it in `{if ...}{/if}`.
