# Ticket Permissions

## Core Roles

The ticket system recognizes these permission sources:

- `Administrator`
  Members with effective administrator permissions always bypass normal ticket action restrictions. Fake permissions should work as well.

- `Server Owner`
  The server owner is treated the same as an administrator for ticket access decisions.

- `Configured Support Roles`
  Support roles are set per ticket option.

- `Configured Staff Roles`
  Staff roles are the global `,settings staff` roles configured for the server.

- `Configured Trainee Roles`
  Trainee roles are set per ticket option and can be granted limited claim/close/speaking behavior depending on that option.

- `Ticket Creator`
  The member who opened the ticket.

- `Ticket Claimer`
  The member currently assigned to the ticket.

## Management Commands

These commands are for server configuration and require `Administrator`:

- `tickets panel`
- `tickets resend`
- `tickets panels`
- `tickets forms`
- `tickets options`
- `tickets profiles`
- `tickets blacklist`

## Profile Commands

- `tickets profile`
  Available to:
  - administrators
  - configured ticket staff
  - configured ticket support
  - configured ticket trainees

- `tickets profiles`
  Admin-only.

## Stats

- `tickets stats`
- `/stats`
  Available to:
  - server owner
  - administrators
  - configured ticket staff
  - configured support roles

## Ticket Actions

## Claim

Who can claim:

- administrators
- configured support roles
- configured staff roles
- configured trainees only if the option allows trainees to claim

Important rules:

- ticket creators cannot claim their own ticket
- trainees can only claim unclaimed tickets
- if a ticket is already claimed, takeover is only possible when that option has `Allow Claim Takeovers` enabled
- trainees cannot reclaim or take over claimed tickets
- support role precedence is higher than trainee
- trainee precedence is higher than staff

## Unclaim

Who can unclaim:

- administrators
- the current claimer
- configured support roles
- configured staff roles

If the ticket is currently claimed by someone else, normal non-staff users cannot unclaim it.

## Close

Who can close:

- administrators
- the ticket creator, if that option has `Creator Can Close` enabled
- the current claimer
- configured support roles
- configured staff roles
- configured trainees if that option allows trainees to close

## Reopen

Who can reopen:

- administrators
- the current claimer
- configured support roles
- configured staff roles

If the ticket is claimed by someone else, only the claimer or admins can reopen it unless the acting member has the configured support/staff access used for ticket actions.

## Delete

Who can delete:

- administrators
- the current claimer
- configured support roles
- configured staff roles

Important rules:

- trainees without action-role coverage cannot delete tickets

## Move

Who can move:

- administrators
- the current claimer
- configured support roles
- configured staff roles

Moving a ticket also requires the member and bot to have the necessary Discord channel management permissions.

## Transcript

Who can generate or fetch a transcript for an active ticket:

- administrators
- the current claimer
- configured support roles
- configured staff roles

For archived transcript lookup by case ID, administrators and the server owner can always access it. If a matching live ticket record exists, normal ticket action rules also apply.

## Rename

Who can rename a ticket:

- administrators
- the current claimer
- configured support roles
- configured staff roles

## Manual Access Changes

These actions include allowing members or roles into a ticket and removing access from them.

Who can manage ticket access:

- administrators
- the current claimer
- configured support roles
- configured staff roles

Hierarchy protections still apply:

- you cannot change your own access
- you cannot change the bot's access
- non-owners cannot target members at or above their top role
- non-owners cannot target roles at or above their top role

## Trainee Permission Management

These actions control trainee-specific ticket overrides.

Who can manage trainee permissions:

- administrators
- configured support roles
- configured staff roles

Hierarchy protections still apply to the target member or role.

## Reason Editing

The `tickets reason` command and `/reason` do not follow the generic ticket action rule exactly.

Who can edit stored reasons:

- administrators
- the current claimer
- configured support roles or staff roles when the claimer is a trainee
- configured support roles or staff roles if the ticket is currently unclaimed

This applies to these stored reasons:

- claim reason
- close reason
- reopen reason
- delete reason

## Creator Visibility After Close

Whether the ticket creator can still see a closed ticket is controlled per option.

If enabled:

- the creator keeps view access after close
- the creator does not automatically get send-message access after close

## Required Roles To Open

Some ticket options may require roles before a user can open them.

This is checked per option, not globally.

If a user does not meet the requirement:

- the ticket is not opened
- the configured denial message is shown ephemerally (invisibly)

## Notes For Server Owners

- Support and trainee roles are option-specific. A member may have access in one ticket type and not another.
- Staff roles are the roles set using ,settings staff. These are used if no option-level support roles are set.
- Claimed tickets are intentionally more restrictive than unclaimed tickets.
