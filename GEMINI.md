# Job Alerts

The `job-alerts` MCP server is a catalog of open internship and new-grad roles
read from employers' own career sites. Use `search_roles` for a question like
"software internships that sponsor visas", `get_role` for one role by id,
and `list_tracks`, `list_groups`, `list_companies` for the values the filters
take. Results default to roles open to US applicants that do not require
citizenship or a clearance; pass `us_only=false`, `country`, or
`include_clearance=true` to widen. Send people to a role's `apply_url`.
