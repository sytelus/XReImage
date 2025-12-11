# Research: X/Twitter Image Requirements & Implementation

## Overview

This document contains research findings on X (formerly Twitter) image requirements and best practices for implementing a client-side image resizer to prevent unnatural cropping in the X/Twitter timeline.

## X/Twitter Image Display Behavior

### The Problem

When images don't match X/Twitter's expected aspect ratios, the platform's UI displays a cropped preview in the timeline. This can:
- Cut off important parts of the image
- Make the image look awkward or unintentional
- Reduce engagement because the full context isn't visible

### Historical Context

X/Twitter previously used a saliency-based algorithm (built on DeepGaze II) to determine crop points:
- Trained on human eye-tracking data
- Predicted "importance" scores for image regions
- Centered crops on the most salient point

However, after bias concerns in 2020 (the algorithm showed preference for light-skinned individuals), X/Twitter changed their approach in 2021:
- Now shows images in their native aspect ratio when possible
- Removed algorithmic cropping for standard aspect ratios
- Provides true preview in Tweet composer

## Recommended Image Dimensions

### Single Image Posts

| Aspect Ratio | Dimensions | Use Case |
|-------------|------------|----------|
| **16:9** | 1200 × 675 px | Best for landscape images |
| **1:1** | 1200 × 1200 px | Square images |
| **4:5** | 1080 × 1350 px | Portrait-friendly |
| **3:4** | 900 × 1200 px | Mobile portrait |

**Key Insight**: The 16:9 aspect ratio (1200×675) is the most versatile and widely recommended.

### Safe Aspect Ratio Range

- **Desktop**: 2:1 to 1:1 display fully without cropping
- **Mobile**: 2:1, 3:4, and 16:9 work best
- **Universal Safe Zone**: Keep critical content centered

### Multi-Image Posts

| Configuration | Aspect Ratio | Notes |
|---------------|--------------|-------|
| 2 images | 7:8 each | Side by side |
| 3 images | Mixed | 1 large (left) + 2 stacked (right) |
| 4 images | 2:1 each | 2×2 grid |

### File Specifications

- **Maximum file size**: 5MB (photos and GIFs)
- **Web upload max**: 15MB
- **Supported formats**: JPEG, PNG, GIF, WebP
- **Recommended format**: PNG for quality, JPEG for smaller files

## Solution Approach

To prevent cropping, we need to:

1. **Letterbox/Pillarbox**: Add padding to images to fit target aspect ratios
2. **Resize**: Scale images to recommended dimensions
3. **Maintain Quality**: Use proper resampling during resize

### Target Dimensions Strategy

| Original Orientation | Target Size | Padding Strategy |
|---------------------|-------------|------------------|
| Landscape (wider) | 1200×675 (16:9) | Pillarbox (top/bottom) if needed |
| Portrait (taller) | 1200×675 (16:9) | Letterbox (left/right) if needed |
| Square | 1200×1200 (1:1) | No padding needed |

## Client-Side Implementation Research

### HTML5 Canvas API

The canvas element is the standard way to manipulate images in the browser:

```javascript
const canvas = document.createElement('canvas');
canvas.width = targetWidth;
canvas.height = targetHeight;
const ctx = canvas.getContext('2d');
ctx.drawImage(image, x, y, width, height);
```

#### Best Practices

1. **Preserve Aspect Ratio**: Calculate scaling factors to maintain proportions
2. **Step-Down Resizing**: For large reductions, resize in steps (halving) for better quality
3. **Quality Control**: Use `canvas.toDataURL('image/jpeg', quality)` or `canvas.toBlob()` with quality parameter

### Clipboard API

Modern browsers support the Clipboard API for copying/pasting images:

**Reading from clipboard:**
```javascript
const items = await navigator.clipboard.read();
for (const item of items) {
    if (item.types.includes('image/png')) {
        const blob = await item.getType('image/png');
        // Process blob
    }
}
```

**Writing to clipboard:**
```javascript
const blob = await fetch(imageUrl).then(r => r.blob());
await navigator.clipboard.write([
    new ClipboardItem({ 'image/png': blob })
]);
```

#### Limitations

- Only PNG format supported for clipboard operations
- Requires HTTPS (secure context)
- Requires user interaction to trigger
- Browser support: Full support since June 2024

### Drag and Drop API

Key implementation points:

1. **Prevent default browser behavior** on `dragover` and `drop` events
2. **Visual feedback** during drag operations
3. **Access files** via `event.dataTransfer.files`

```javascript
dropZone.addEventListener('drop', (e) => {
    e.preventDefault();
    const files = e.dataTransfer.files;
    // Process files
});
```

### File Input Fallback

Always provide traditional file input as fallback:

```html
<input type="file" accept="image/*" />
```

## UI/UX Design Research

### Modern Design Trends (2024-2025)

#### Glassmorphism
- Frosted glass appearance
- Semi-transparent backgrounds
- Blur effects (`backdrop-filter: blur()`)
- Subtle borders
- Works well with colorful backgrounds

**CSS Example:**
```css
.glass-panel {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 16px;
}
```

#### Considerations
- Performance: Blur effects can be GPU-intensive
- Accessibility: Ensure sufficient contrast
- Fallbacks: Provide solid backgrounds for unsupported browsers

## Security Considerations

1. **Client-side only**: No server uploads required
2. **File type validation**: Accept only image MIME types
3. **Size limits**: Validate file sizes before processing
4. **No external dependencies**: Reduces attack surface

## Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| Canvas API | ✓ | ✓ | ✓ | ✓ |
| Clipboard API (read) | 76+ | 127+ | 13.1+ | 79+ |
| Clipboard API (write) | 66+ | 127+ | 13.1+ | 79+ |
| Drag & Drop | ✓ | ✓ | ✓ | ✓ |
| backdrop-filter | 76+ | 103+ | 9+ | 79+ |

## Sources

- [X (Twitter) Image Dimensions 2024](https://supapost.ai/blog/best-practices/x-twitter-image-dimensions-2024)
- [Twitter Image Specs Guide 2025](https://soona.co/image-resizer/twitter-spec-guide)
- [RecurPost Twitter Dimensions](https://recurpost.com/blog/twitter-post-dimensions/)
- [X Engineering: Image Cropping Algorithm](https://blog.x.com/engineering/en_us/topics/insights/2021/sharing-learnings-about-our-image-cropping-algorithm)
- [MDN: Clipboard API write()](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/write)
- [MDN: Clipboard API read()](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard/read)
- [MDN: File Drag and Drop](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API/File_drag_and_drop)
- [web.dev: Copy Images to Clipboard](https://web.dev/patterns/clipboard/copy-images)
- [ImageKit: Resize Images in JavaScript](https://imagekit.io/blog/how-to-resize-image-in-javascript/)
- [MIT SDE: Neumorphism vs Glassmorphism](https://blog.mitsde.com/2024-ui-design-trends-neumorphism-vs-glassmorphism-explained/)
