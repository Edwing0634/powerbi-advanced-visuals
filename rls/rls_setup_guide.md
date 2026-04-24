# Row-Level Security Setup Guide

## Overview

This report uses a 4-level role hierarchy enforced via Power BI RLS:

```
Admin
  └── Senior
        └── GA (Agency Manager)
              └── GU (Unit Manager)
```

## Required Table Columns

### dim_agent
| Column | Type | Description |
|--------|------|-------------|
| `agent_id` | Integer | Primary key |
| `full_name` | Text | Display name |
| `email` | Text | Login email (must match UPN) |
| `role` | Text | Admin / Senior / GA / GU |
| `unit_name` | Text | Foreign key to dim_unit |
| `supervisor_email` | Text | Email of supervising Senior |
| `group_manager_email` | Text | Email of GU manager |

### dim_unit
| Column | Type | Description |
|--------|------|-------------|
| `unit_name` | Text | Primary key |
| `unit_manager_email` | Text | Email of GA manager |

## Setup Steps

1. Open Power BI Desktop → **Modeling** tab → **Manage Roles**
2. Create four roles: `Admin`, `Senior`, `GA`, `GU`
3. Apply the DAX filters from `role_definitions.dax` to each role
4. Publish to Power BI Service
5. Go to **Dataset Settings → Security** → assign users/groups to roles
6. Test with **"View As Role"** in Desktop using test email addresses

## Testing Locally

```
View As Role → GA → enter: manager@yourdomain.com
Expected: only dim_unit rows where unit_manager_email = 'manager@yourdomain.com'
```

## Common Pitfalls

- UPN in Power BI Service must match exactly the email stored in the table (case-insensitive)
- If using Azure AD groups, assign the group to the role — individual members inherit it
- `USERPRINCIPALNAME()` returns empty string in Desktop unless using "View As Role"
