# XReImage

**Resize images for perfect display on X/Twitter — no more awkward cropping**

XReImage is a free, open-source, client-side tool that helps you prepare images for X (formerly Twitter) so they display perfectly in the timeline without being cropped awkwardly.

## The Problem

When you post an image on X/Twitter that doesn't match the platform's expected aspect ratios, the timeline shows a cropped preview. This can:

- Cut off important parts of your image
- Make screenshots or infographics look incomplete
- Reduce engagement because viewers can't see the full context

## The Solution

XReImage adds letterboxing or pillarboxing (padding) around your images to fit X/Twitter's preferred dimensions while keeping your entire image visible. The result is a perfectly framed image that displays fully in the timeline.

## Features

- **Multiple input methods**: Paste from clipboard, upload, or drag and drop
- **Multiple output methods**: Copy to clipboard, download, or drag to other apps
- **Aspect ratio presets**: 16:9 (recommended), 1:1, and 4:3
- **Background options**: Transparent, solid color, or blurred image fill
- **Border customization**: Color, width, and corner radius
- **Drop shadow**: Adjustable blur and opacity
- **Export formats**: PNG, JPEG, WebP with quality control
- **100% client-side**: Your images never leave your browser
- **No installation**: Works directly in your web browser

## Quick Start (For Everyone)

### Using the Online Tool

1. Open `index.html` in your web browser
2. Add your image using any of these methods:
   - **Paste**: Press `Ctrl+V` (Windows/Linux) or `Cmd+V` (Mac)
   - **Upload**: Click the upload area and select a file
   - **Drag & Drop**: Drag an image file onto the upload area
3. Your image appears in the preview
4. (Optional) Click "Customize Output" to adjust settings
5. Export your image:
   - **Copy**: Click "Copy to Clipboard" then paste directly into X/Twitter
   - **Download**: Click "Download" to save the file
   - **Drag**: Drag the processed image to another application

### Recommended Settings for X/Twitter

| Setting | Recommended Value | Why |
|---------|-------------------|-----|
| Aspect Ratio | 16:9 | Best for timeline display |
| Background | Blur or Color | Looks professional |
| Padding | 20px | Prevents edge clipping |
| Shadow | Enabled | Adds depth and polish |
| Format | PNG | Best quality for screenshots |
| Format | JPEG (85%) | Best for photos |

## Detailed Guide

### Input Methods Explained

#### Clipboard Paste (Easiest)
1. Take a screenshot or copy an image from any application
2. Go to XReImage in your browser
3. Press `Ctrl+V` (or `Cmd+V` on Mac)
4. The image loads automatically

#### File Upload
1. Click anywhere in the upload zone
2. Select an image file from your computer
3. Supported formats: JPEG, PNG, WebP, GIF

#### Drag and Drop
1. Open your file explorer/finder
2. Drag an image file onto the XReImage upload zone
3. Drop when the zone highlights

### Customization Options

#### Aspect Ratio
- **16:9** (1200×675): Standard for tweets, best compatibility
- **1:1** (1200×1200): Square format, great for centered content
- **4:3** (1200×900): Classic ratio, good for presentations

#### Background Types
- **Transparent**: Shows the checkered pattern (only visible with PNG export)
- **Color**: Pick any solid color using the color picker
- **Blur**: Creates a frosted glass effect using your image

#### Border
- Enable to add a frame around your image
- Customize the color, width (1-20px), and corner radius

#### Shadow
- Adds depth to your image
- Adjust blur amount and opacity for subtle or dramatic effects

#### Export Format
- **PNG**: Lossless, best for screenshots, supports transparency
- **JPEG**: Smaller files, best for photos, no transparency
- **WebP**: Modern format, good compression, moderate support

### Output Methods

#### Copy to Clipboard
- Click "Copy to Clipboard"
- Paste directly into X/Twitter's compose window
- Also works with Discord, Slack, emails, etc.

#### Download
- Click "Download"
- File saves to your downloads folder
- Filename format: `xreimage-[timestamp].[format]`

#### Drag Export
- Drag the processed image directly from the preview
- Drop into X/Twitter, file explorer, or other applications
- Browser support varies

## Technical Details (For Developers)

### Architecture

XReImage is a single HTML file containing:
- HTML5 structure
- Inline CSS (glassmorphism design)
- Inline JavaScript (vanilla, no frameworks)

### Key Technologies

- **HTML5 Canvas API**: All image processing
- **Clipboard API**: Read/write images
- **File API**: Handle uploads
- **Drag and Drop API**: Input and output

### Image Processing Pipeline

1. **Load**: FileReader → Image element
2. **Calculate**: Fit dimensions maintaining aspect ratio
3. **Render**: Canvas with background → shadow → image → border
4. **Export**: canvas.toBlob() or canvas.toDataURL()

### Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| Canvas API | ✓ | ✓ | ✓ | ✓ |
| Clipboard read | 76+ | 127+ | 13.1+ | 79+ |
| Clipboard write | 66+ | 127+ | 13.1+ | 79+ |
| backdrop-filter | 76+ | 103+ | 9+ | 79+ |

### File Structure

```
XReImage/
├── index.html      # Main application (single file)
├── README.md       # This documentation
├── RESEARCH.md     # Research findings on X/Twitter requirements
├── REQUIREMENTS.md # Project requirements
├── PLAN.md         # Implementation plan
├── LICENSE         # MIT License
└── .gitignore      # Git ignore rules
```

### Hosting

Deploy by placing `index.html` on any static hosting:
- GitHub Pages
- Netlify
- Vercel
- Any web server
- Open directly from local file system

### Contributing

1. Fork the repository
2. Make changes to `index.html`
3. Test in multiple browsers
4. Submit a pull request

### Code Style

- All code in single file for simplicity
- Well-commented for maintainability
- No external dependencies
- ES6+ JavaScript (IIFE pattern)
- BEM-inspired CSS class naming

## FAQ

### Why doesn't X/Twitter just show images properly?

X/Twitter optimizes timeline display for speed and consistency. Images outside certain aspect ratios get cropped to fit their layout. This tool helps you work with their system rather than against it.

### Will my images be uploaded anywhere?

No. All processing happens in your browser using the Canvas API. Your images never leave your computer.

### What's the maximum image size?

The tool accepts images up to 15MB. X/Twitter's limit is 5MB for most uploads (15MB via web), so keep that in mind.

### Can I use this for other social media?

Yes! While optimized for X/Twitter, the 16:9 ratio works well for many platforms including LinkedIn and Facebook.

### Why PNG for clipboard?

The Clipboard API only supports PNG format reliably across browsers. When you copy to clipboard, it's always PNG regardless of your export format setting.

## Privacy

XReImage is completely private:
- No data collection
- No analytics
- No cookies
- No server communication
- 100% client-side processing

Your images stay on your computer.

## License

MIT License — free to use, modify, and distribute.

## Credits

Built with vanilla HTML, CSS, and JavaScript. No frameworks, no dependencies.

---

**Happy posting!** If XReImage helps you, consider starring the repository.
