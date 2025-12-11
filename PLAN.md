# Implementation Plan: XReImage

## Overview

Build a single-page HTML application that resizes images for optimal display on X/Twitter, preventing unnatural cropping in the timeline.

## Architecture

```
index.html (single file)
├── HTML Structure
│   ├── Header with title and description
│   ├── Input zone (drag/drop/paste/upload)
│   ├── Preview area (before/after)
│   ├── Options panel
│   └── Output zone (copy/download/drag)
├── CSS Styles
│   ├── CSS Variables for theming
│   ├── Glassmorphism effects
│   ├── Responsive layout
│   └── Animations/transitions
└── JavaScript
    ├── Image input handlers
    ├── Canvas processing
    ├── Options management
    └── Output handlers
```

## Implementation Steps

### Phase 1: HTML Structure

1. **Document setup**
   - HTML5 doctype
   - Meta tags (viewport, charset, description)
   - Inline CSS in `<style>` tag
   - Inline JavaScript in `<script>` tag

2. **Layout sections**
   - Header: Logo, title, brief description
   - Drop zone: Large clickable/droppable area
   - Preview container: Side-by-side original/processed
   - Options panel: Collapsible settings
   - Output area: Action buttons

### Phase 2: CSS Styling

1. **Design system**
   ```css
   :root {
     --primary: #1DA1F2;  /* Twitter blue */
     --background: #0f0f1a;
     --surface: rgba(255,255,255,0.05);
     --text: #ffffff;
     --border-radius: 16px;
   }
   ```

2. **Key components**
   - Glassmorphism panels with backdrop-filter
   - Gradient backgrounds
   - Smooth transitions
   - Drag state indicators

### Phase 3: JavaScript - Input Handling

1. **Paste handler**
   ```javascript
   document.addEventListener('paste', async (e) => {
     const items = e.clipboardData.items;
     for (const item of items) {
       if (item.type.startsWith('image/')) {
         const blob = item.getAsFile();
         loadImage(blob);
       }
     }
   });
   ```

2. **File input handler**
   ```javascript
   fileInput.addEventListener('change', (e) => {
     const file = e.target.files[0];
     if (file) loadImage(file);
   });
   ```

3. **Drag and drop handlers**
   - `dragenter`: Show visual feedback
   - `dragover`: Prevent default, continue feedback
   - `dragleave`: Remove feedback
   - `drop`: Process dropped file

### Phase 4: JavaScript - Image Processing

1. **Load image into canvas**
   ```javascript
   function loadImage(blob) {
     const img = new Image();
     img.onload = () => processImage(img);
     img.src = URL.createObjectURL(blob);
   }
   ```

2. **Process image**
   ```javascript
   function processImage(img) {
     const canvas = document.createElement('canvas');
     canvas.width = targetWidth;  // 1200
     canvas.height = targetHeight; // 675 for 16:9
     const ctx = canvas.getContext('2d');

     // Draw background
     drawBackground(ctx, options);

     // Calculate fit dimensions
     const {x, y, w, h} = calculateFit(img, canvas);

     // Draw image centered
     ctx.drawImage(img, x, y, w, h);

     // Apply effects
     applyEffects(ctx, options);
   }
   ```

3. **Background options**
   - Transparent: Clear canvas
   - Solid color: Fill with selected color
   - Blur: Draw scaled/blurred version of source image

### Phase 5: JavaScript - Output Handling

1. **Copy to clipboard**
   ```javascript
   async function copyToClipboard() {
     const blob = await new Promise(r =>
       canvas.toBlob(r, 'image/png')
     );
     await navigator.clipboard.write([
       new ClipboardItem({'image/png': blob})
     ]);
   }
   ```

2. **Download**
   ```javascript
   function download() {
     const link = document.createElement('a');
     link.download = `xreimage-${Date.now()}.${format}`;
     link.href = canvas.toDataURL(`image/${format}`, quality);
     link.click();
   }
   ```

3. **Drag from page**
   - Make output canvas draggable
   - Set dataTransfer with blob URL

### Phase 6: Options UI

1. **Aspect ratio selector**
   - 16:9 (1200×675) - Recommended for tweets
   - 1:1 (1200×1200) - Square
   - 4:3 (1200×900) - Classic

2. **Background type**
   - Radio buttons: Transparent / Color / Blur
   - Color picker for solid color
   - Blur intensity slider

3. **Border settings**
   - Checkbox to enable
   - Color picker
   - Width slider (1-20px)

4. **Shadow settings**
   - Checkbox to enable
   - Blur radius slider
   - Color picker with alpha

5. **Export settings**
   - Format dropdown: PNG, JPEG, WebP
   - Quality slider (for JPEG/WebP)

## File Structure

```
XReImage/
├── index.html          # Main application (single file)
├── README.md           # User documentation
├── RESEARCH.md         # Research findings
├── REQUIREMENTS.md     # Project requirements
├── PLAN.md            # This file
├── LICENSE            # MIT License
└── .gitignore         # Git ignore rules
```

## Testing Checklist

- [ ] Paste image from clipboard
- [ ] Upload via file picker
- [ ] Drag and drop image
- [ ] Copy result to clipboard
- [ ] Download result
- [ ] Drag result to other apps
- [ ] Each aspect ratio option
- [ ] Each background type
- [ ] Border on/off with customization
- [ ] Shadow on/off with customization
- [ ] Each export format
- [ ] Quality slider affects file size
- [ ] Works in Chrome, Firefox, Safari, Edge
- [ ] Responsive on mobile devices

## Performance Considerations

1. Use `requestAnimationFrame` for live preview updates
2. Debounce slider inputs
3. Use `OffscreenCanvas` if available
4. Revoke object URLs when no longer needed
5. Limit blur calculations for large images

## Accessibility

1. Keyboard navigation for all controls
2. ARIA labels on interactive elements
3. Focus indicators
4. Color contrast meeting WCAG AA
5. Screen reader announcements for state changes
