# CSS Design Tokens for DAX HTML Measures

Consistent values to copy-paste into DAX string concatenations.

## Colors

| Token | Value | Usage |
|-------|-------|-------|
| `--color-success` | `#28a745` | Achievement ≥ 100% |
| `--color-warning` | `#ffc107` | Achievement 80–99% |
| `--color-danger` | `#dc3545` | Achievement < 80% |
| `--color-primary` | `#0d6efd` | Accent, links |
| `--color-muted` | `#6c757d` | Labels, subtitles |
| `--color-bg` | `#f8f9fa` | Card backgrounds |
| `--color-border` | `#dee2e6` | Table dividers |

## Typography (inline)

```css
/* KPI label */
font-size:11px; color:#6c757d; text-transform:uppercase; letter-spacing:.5px;

/* Main metric value */
font-size:32px; font-weight:700; color:#212529; line-height:1;

/* Table header */
font-size:11px; color:#6c757d; text-transform:uppercase; font-weight:600;

/* Table cell */
font-size:13px; color:#212529; padding:7px 10px;
```

## Badge / Pill

```css
border-radius:20px; padding:2px 10px; font-size:12px; font-weight:600; color:#fff;
background: #28a745;  /* swap color per semaphore */
```

## Card Container

```css
border-radius:12px;
border-left:6px solid #28a745;  /* swap color per semaphore */
padding:14px 18px;
box-shadow:0 2px 8px rgba(0,0,0,.08);
background:#fff;
font-family:Segoe UI, sans-serif;
```

## Notes

- All styles must be **inline** — the HTML Content visual's iframe blocks `<style>` tags in some versions.
- Use `font-family:Segoe UI,sans-serif` to match the Power BI native font.
- `overflow:auto` on table wrappers prevents clipping in narrow visuals.
