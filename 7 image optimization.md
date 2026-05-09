# Image Optimization Notes

# 1. Responsive Images

Responsive images allow browsers to load different image resolutions based on:
- Screen size
- Device pixel density
- Device capability

Example:

```html
<img
  alt="Baby"
  src="baby-lowres.jpg"
  srcset="
    baby-highres.jpg 2x,
    baby-high-3.jpg 3x
  "
/>
```

## How It Works

- `1x` → normal screens
- `2x` → retina displays
- `3x` → ultra high density displays

Browser automatically selects the best image.

---

# 2. Progressive Enhancement (Image Formats)

Serve modern image formats first and provide fallback support.

Example:

```html
<picture>
  <source srcset="photo.avif" type="image/avif">
  <source srcset="photo.webp" type="image/webp">
  <img src="photo.jpg" alt="Photo">
</picture>
```

## Browser Flow

1. Try AVIF
2. If unsupported → try WebP
3. If unsupported → fallback JPEG

## Benefits

- Smaller file sizes
- Faster loading
- Better performance
- Backward compatibility

---

# 3. Adaptive Media Loading

Adaptive loading means:
> Serve media according to network speed and device capability.

Examples:
- Slow internet → low quality images
- Fast internet → HD videos
- Weak device → fewer animations

---

# 4. navigator.connection

Provides network information.

Example:

```js
console.log(navigator.connection.effectiveType)
```

Possible values:
- `slow-2g`
- `2g`
- `3g`
- `4g`

---

# 5. navigator.connection.saveData

Checks whether user enabled data saver mode.

Example:

```js
if (navigator.connection.saveData) {
  console.log("Reduce heavy content")
}
```

## Use Cases

- Disable autoplay videos
- Compress images
- Reduce network requests

---

# 6. navigator.deviceMemory

Returns approximate RAM available on device.

Example:

```js
console.log(navigator.deviceMemory)
```

Possible outputs:
- 1
- 2
- 4
- 8

## Use Cases

- Disable heavy animations
- Reduce image quality
- Avoid rendering huge lists

---

# 7. navigator.hardwareConcurrency

Returns number of CPU cores.

Example:

```js
console.log(navigator.hardwareConcurrency)
```

## Use Cases

- Limit parallel processing
- Reduce heavy tasks
- Improve performance on weak CPUs

---

# 8. Advanced Real World Use Cases

## Disable Video on Slow Network

```js
const connection = navigator.connection;

if (connection.effectiveType.includes("2g")) {
  // Replace video with image
}
```

---

## Limit Carousel on Slow 3G

```js
if (connection.effectiveType === "3g") {
  loadOnlyFirstFewSlides();
}
```

Benefits:
- Faster loading
- Reduced bandwidth usage

---

# 9. Blur Placeholder Technique (Medium Style)

Initially load:
- Tiny blurred image
- Replace later with HD image

Flow:

```text
Blurred Image → Actual HD Image
```

Benefits:
- Better perceived performance
- Avoid blank loading spaces

Example:

```html
<img
  src="tiny-blur.jpg"
  data-src="actual-image.jpg"
/>
```

---

# 10. Solid Primary Color Placeholder

Instead of empty white space:
- Extract dominant image color
- Use it as temporary background

Examples:
- Ocean image → blue placeholder
- Forest image → green placeholder

Benefits:
- Better UX
- Reduces layout flashing

---

# 11. Image Sprites

Old optimization technique.

Instead of loading:
- 10 separate icons

Combine them into:
- 1 sprite image

Example:

```css
.icon {
  background-image: url(sprite.png);
  background-position: -20px -40px;
}
```

## Benefits

- Fewer HTTP requests
- Faster loading

## Modern Alternatives

- SVG icons
- Inline SVG
- Icon libraries

---

# 12. Lazy Loading

Load images only when they enter viewport.

Example:

```html
<img src="photo.jpg" loading="lazy" />
```

## Benefits

- Faster initial load
- Reduced bandwidth
- Better performance

Useful for:
- Ecommerce sites
- Blogs
- Infinite scrolling feeds

---

# 13. Difference Between Concepts

## Lazy Loading vs Responsive Images

| Feature | Purpose |
|---|---|
| Lazy Loading | Delay loading |
| Responsive Images | Load correct image size |

---

## Progressive Enhancement vs Adaptive Loading

| Feature | Purpose |
|---|---|
| Progressive Enhancement | Browser capability based |
| Adaptive Loading | Device/network based |

---

# 14. Quick Summary

| Technique | Goal |
|---|---|
| Responsive Images | Correct image size |
| Lazy Loading | Delay loading |
| AVIF/WebP | Better compression |
| Blur Placeholder | Better perceived speed |
| Adaptive Loading | Device-aware optimization |
| Image Sprites | Reduce HTTP requests |
| saveData API | Respect user bandwidth |
| deviceMemory | Optimize for weak devices |
| hardwareConcurrency | CPU-aware optimization |

---

# 15. One-Line Interview Answers

## Responsive Images
Serve different image resolutions based on screen density and device capability.

## Lazy Loading
Load media only when it becomes visible in viewport.

## Progressive Enhancement
Serve modern optimized image formats with fallback support.

## Adaptive Loading
Optimize content dynamically based on network and hardware capability.

## Image Sprites
Combine multiple icons into one image to reduce network requests.
