# Example Html Frontend Structure

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Several raw projects render through `views/**` but load their frontend assets from `html/**`.
- Without this pattern, the agent may inspect only Blade files and miss the real CSS, JS, images, and fonts.

## Example Shape

```blade
<link rel="stylesheet" href="html/css/vendor.css">
<link rel="stylesheet" href="html/css/main.min.css">
<link rel="stylesheet" href="html/css/custom.css?v=2">

<script defer src="html/js/libs/swiper.js"></script>
<script defer src="html/js/main.js"></script>
<script defer src="html/js/custom.js"></script>

<img src="html/img/icons/close-btn.svg" alt="">
<link rel="preload" href="html/fonts/Inter-Regular.woff2" as="font" type="font/woff2" crossorigin>
```

## Working Rule

- Treat `views/**` and the linked `html/**` directory as one implementation unit.
- If active layout files reference `html/css`, `html/js`, `html/img`, or `html/fonts`, keep that directory in git and include it in analysis.
- Do not refactor a view before confirming where its frontend assets actually live.

## Verify On Live Project

- active layout file
- linked asset directory
- whether `html/` is source, build output, or both
- whether another directory such as `assets/` also participates in the same frontend flow
